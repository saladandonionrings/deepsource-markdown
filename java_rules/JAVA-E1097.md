# Avoid throwing `null`
**ID:** `JAVA-E1097` | **Lien:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-E1097)

![Critical](https://img.shields.io/badge/severity-critical-red) ![Bug Risk](https://img.shields.io/badge/type-bug_risk-green)

Throwing `null` is a bad practice and should be avoided, as it serves no meaningful purpose.

Throwing a literal `null` value will cause Java to throw a `NullPointerException` instead.


## Bad Practice

```java
throw null;
```

## Recommended
If you need to throw a `NullPointerException` , do so directly.


```java
throw new NullPointerException("something was null!");
```
