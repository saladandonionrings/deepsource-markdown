# Possibly null fields should not be synchronized on
**ID:** `JAVA-E1060` | **Link:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-E1060)

![Critical](https://img.shields.io/badge/severity-critical-red)![Bug Risk](https://img.shields.io/badge/type-bug_risk-green)

There is a null check for a field which is being synchronized on. Since the field is synchronized on, it is unlikely that it will be `null`. If it is `null` and then synchronized on, a `NullPointerException` will be thrown on synchronization and the check would be pointless.


## Bad Practice

```java
// may throw NPE.
synchronize(possiblyNull) {
    if (possiblyNull != null) {
        // ...
    }
}
```

## Recommended

```java
synchronize(nonNull) {
    if (possiblyNull != null) {
        // ...
    }
}
```
Avoid structuring your code in a way that allows for nullable values to be used in a non-null context.

