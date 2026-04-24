# `Boolean` method may return `null`
**ID:** `JAVA-S0030` | **Link:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-S0030)

![Major](https://img.shields.io/badge/severity-major-orange)![Bug Risk](https://img.shields.io/badge/type-bug_risk-green)

A method with a `Boolean` return type returns an explicit null. This is likely intentional, but be aware that API consumers may not realize that.


## Examples

## Bad Practice

```java
public Boolean checkSomething() {
    if (something) {
        boolean boolValue = ...;
        return boolValue;
    } else return null;
}
```
If this is intended, such as when interfacing with a database, ensure that the behavior is well documented to avoid confusion. In most cases, it is better to use the native `boolean` type instead of the `Boolean` wrapper type to avoid accidents.

A better alternative would be to use the [`Optional`](https://docs.oracle.com/javase/8/docs/api/java/util/Optional.html) type introduced in Java 8.


## Recommended

```java
public Optional<Boolean> checkSomething() {
    if (something) {
        boolean boolValue = ...;
        return Optional.of(boolValue);
    } else return Optional.empty();
}
```
