# Useless unboxing of a value
**ID:** `JAVA-W1052` | **Lien:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-W1052)

![Minor](https://img.shields.io/badge/severity-minor-yellow) ![Performance](https://img.shields.io/badge/type-performance-white)

A boxed value is unboxed and then immediately reboxed. This has likely occurred due to an unboxing operation by the programmer, which the java compiler has undone.


## Bad Practice

```java
public void acceptsInteger(Integer x) { ... }

Integer a = 0;
Float b = 1f;

// Same type, unboxed.
acceptsInteger(a.intValue());

// Different type, unboxed.
acceptsInteger(b.intValue());
```
The bytecode for the first call would look something like:


```java
aload_1                           // Load `a` into memory.
invokevirtual #13                 // Call instance method `Integer.intValue()` on `a`
invokestatic  #7                  // Call static method `Integer.valueOf()` with the result of `intValue()`
invokestatic  #23                 // Call instance method `acceptsInteger()`
```
Note two contradicting method calls: this code calls `Integer.intValue()` immediately followed by `Integer.valueOf()` .


## Recommended
If a method expects a boxed type, it is better to provide it a value that is of the same type than to give it one of a different one.


```java
acceptsInteger(a);
```
