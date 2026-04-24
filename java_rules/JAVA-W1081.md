# Primitive value is boxed, then unboxed to perform primitive coercion
**ID:** `JAVA-W1081` | **Lien:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-W1081)

![Major](https://img.shields.io/badge/severity-major-orange) ![Anti-pattern](https://img.shields.io/badge/type-anti_pattern-purple)

A boxed value is constructed and then immediately converted into a different primitive type.

Consider performing a direct cast instead.


## Bad Practice
Wrapping a primitive and then unwrapping it immediately after is a redundant operation that unnecessarily creates a new object.


```java
int value = new Double(d).intValue();
```

## Recommended
It is better to just cast the primitive to the desired type directly instead.


```java
int value = (int) d;
```
