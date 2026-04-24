# Arguments of binary expressions must not be duplicated
**ID:** `JAVA-E0034` | **Lien:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-E0034)

![Major](https://img.shields.io/badge/severity-major-orange) ![Bug Risk](https://img.shields.io/badge/type-bug_risk-green)

The code contains an expression that appears twice, one right after the other.

The expression does not appear to be a part of any operations such as exponentiation, and may indicate a typo.


## Bad Practice

```java
x == 0 || x == 0

(x + 1) / (x + 1)
```
Perhaps the second occurrence is intended to be something else.


## Recommended

```java
x == 0 || y == 0


(x + 1) / (y + 1)
```
Double check if this was intended; it likely wasn't.


## Exceptions
This issue will not be raised when the operator is `*` , as usually that would indicate an exponentiation operation.

