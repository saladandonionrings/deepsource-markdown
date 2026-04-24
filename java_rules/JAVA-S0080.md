# Incorrect combination of `Math.max` and `Math.min`
**ID:** `JAVA-S0080` | **Lien:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-S0080)

![Critical](https://img.shields.io/badge/severity-critical-red) ![Bug Risk](https://img.shields.io/badge/type-bug_risk-green)

This code tries to limit the value bounds using a construct such as:


```java
Math.min(0, Math.max(100, value))
```
However, the order of the constants is incorrect. It should actually be:


```java
Math.min(100, Math.max(0, value))
```
As the result this code always produces the same result (the constant in min or NaN if the value is NaN).

