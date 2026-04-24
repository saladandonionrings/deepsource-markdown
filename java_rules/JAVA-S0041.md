# Float precision math may be imprecise
**ID:** `JAVA-S0041` | **Lien:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-S0041)

![Minor](https://img.shields.io/badge/severity-minor-yellow) ![Anti-pattern](https://img.shields.io/badge/type-anti_pattern-purple)

The method performs math operations using floating point precision. Floating point precision is very imprecise.


## Examples

## Bad Practice

```java
assert 16777216.0f + 1.0f == 16777216.0f; // This will not trigger an error!
```
Consider using `double` math instead.


## Recommended

```java
assert 16777216.0 + 1.0 == 16777217.0;
```
