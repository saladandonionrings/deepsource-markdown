# Boxed Boolean values should not be used in conditional expressions
**ID:** `JAVA-E1054` | **Link:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-E1054)

![Major](https://img.shields.io/badge/severity-major-orange)![Bug Risk](https://img.shields.io/badge/type-bug_risk-green)

A boxed boolean value (`java.lang.Boolean`) is being used in a potentially dangerous manner. Such usage may lead to a `NullPointerException` being thrown.

Java's primitive types differ from normal classes in that primitives can never be null. However, classes can be null, even primitive wrapper classes such as `Boolean` or `Character`. For `Boolean` in particular, this means that turning a `boolean` into a `Boolean` promotes it from a binary `true` or `false` to a ternary `true`, `false` or `null`. This may be undesirable in most cases, and is worth avoiding.

The nullability of wrapper types becomes an issue when the wrapper types are used directly in expressions without null checks.


## Bad Practice
In the example below, the `if` statement evaluates the value of `nullable` when its value is `null`. This will lead to a `NullPointerException` being thrown.


```java
Boolean nullable = null

if (nullable) { // This would throw a NullPointerException.
    // ...
}
```

## Recommended
You could perform an explicit comparison with the required value.


```java
if (nullable == true) {
    // ...
}

// OR

if (Boolean.TRUE.equals(nullable)) {
    // ...
}
```
Using `Boolean.TRUE` here can prevent an unnecessary unboxing conversion when performing a comparison.

