# Avoid using `equals` to compare against `null`
**ID:** `JAVA-E0051` | **Link:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-E0051)

![Critical](https://img.shields.io/badge/severity-critical-red)![Bug Risk](https://img.shields.io/badge/type-bug_risk-green)

Tests for null should not use the `equals` method. The '==' operator should be used instead.

Consider this string declaration:


```java
String x = "foo";
```

## Bad Practice

```java
if (x.equals(null)) {
    doSomething();
}
```
If `x` is null in the above snippet, calling `equals` on it would result in a `NullPointerException`.


## Recommended

```java
if (x == null) {
    doSomething();
}
```
Since the `==` operator directly compares references, it does not have the same problem.

