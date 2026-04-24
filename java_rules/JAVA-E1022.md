# `Optional` values must never be `null`
**ID:** `JAVA-E1022` | **Link:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-E1022)

![Critical](https://img.shields.io/badge/severity-critical-red)![Bug Risk](https://img.shields.io/badge/type-bug_risk-green)

`Optional<T>` is a type that serves to help developers avoid one of the oldest mistakes in the book: `NullPointerException`s. Due to Java's unfortunate choice of semantics however, the wonderful properties of this type are overshadowed by the fact that variables which refer to `Optional` objects themselves can still hold null values.

Storing `null` into an `Optional` variable is not recommended and must be strictly avoided.


## Bad Practice
Avoid returning null in functions that return an `Optional` value.


```java
Optional<MyClass> method() {
    if (new Random().nextBoolean()) return null; // Don't return null!
    else return Optional.of(new MyClass());
}
```
Do not assign `null` to an `Optional` value:


```java
Optional<Integer> o = null;
```

## Recommended
If you wish to construct an `Optional` that holds no value, use `Optional.empty()` instead of assigning a null value.


```java
Optional<MyClass> method() {
    if (new Random().nextBoolean()) return Optional.empty();
    else return Optional.of(new MyClass());
}
```
If you wish to convert a value which may be null into an `Optional` use `Optional.ofNullable()`:


```java
String s = null;

Optional<String> opt = Optional.ofNullable(s);

opt.isEmpty(); // true
```
