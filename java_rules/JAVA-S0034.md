# Repeated conditional test on the same variable detected
**ID:** `JAVA-S0034` | **Link:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-S0034)

![Major](https://img.shields.io/badge/severity-major-orange)![Bug Risk](https://img.shields.io/badge/type-bug_risk-green)

The code contains a conditional test that is performed twice, one right after the other.


## Example

## Bad Practice

```java
x == 0 || x == 0
```
Perhaps the second occurrence is intended to be something else.


## Recommended

```java
x == 0 || y == 0
```
Double check if this was intended; it likely wasn't.

