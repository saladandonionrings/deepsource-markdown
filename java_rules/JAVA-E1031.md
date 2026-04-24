# `Double`/`Float` comparison with `NaN` will always return `false`
**ID:** `JAVA-E1031` | **Link:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-E1031)

![Major](https://img.shields.io/badge/severity-major-orange)![Bug Risk](https://img.shields.io/badge/type-bug_risk-green)

Avoid comparing floating point values to `Float.NaN` or `Double.NaN`, as such comparisons will always return `false`.

This occurs due to the special semantics of `NaN`; it is equal to nothing, not even itself[.](https://pbs.twimg.com/media/BtgKyZOCcAAA_ED?format=jpg&name=900x900)

The [Java Language Specification](https://docs.oracle.com/javase/specs/jls/se11/html/jls-4.html#jls-4.2.3) states the following on the subject:

The equality operator `==` returns `false` if either operand is `NaN`.


## Bad Practice

```java
if (x == Double.NaN) { ... } // This condition will always fail.

if (y == Float.NaN) { ... } // This will also fail.

assertEquals(Double.NaN, Double.NaN); // Fails: NaN != NaN
```

## Recommended
Use the `isNaN()` method to check if a floating point value is `NaN` instead of comparing the value with `NaN`. `isNaN()` is available as both a static method and as an instance method.


```java
if (Double.isNaN(x) || x.isNaN()) { ... }

// Alternatively for floats:
if (Float.isNaN(x) || x.isNaN()) { ... }
```
