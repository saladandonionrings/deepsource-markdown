# Method with `Optional` return type must not return `null`
**ID:** `JAVA-S0031` | **Lien:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-S0031)

![Critical](https://img.shields.io/badge/severity-critical-red) ![Bug Risk](https://img.shields.io/badge/type-bug_risk-green)

The usage of an `Optional` return type ( `java.util.Optional` or `com.google.common.base.Optional` for Java 7) always means that explicit `null` returns were not desired by design.


## Examples

## Bad Practice

```java
public Optional<Boolean> checkSomething() {
    Optional<Boolean> retVal = null;

    if (something) {
        boolean boolValue = ...;
        retVal = Optional.of(boolValue);
    }

    return retVal; // May be null!
}
```
Returning a null value in such a case is a contract violation and will most likely break client code. In addition, this introduces the danger of encountering a null pointer exception in scenarios which expressly wish to prevent them.

Always initialize `Optional` s with the value returned by `Optional.empty()` instead of initializing to `null` :


## Recommended

```java
Optional<Boolean> retVal = Optional<>.empty();

if (something) {
    boolean boolValue = ...;
    retVal = Optional.of(boolValue);
}

return retVal; // retVal is now either empty or boolean valued; never null.
```
