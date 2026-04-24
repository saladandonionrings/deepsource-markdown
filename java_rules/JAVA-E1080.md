# The `%` operator has higher precedence than the `*` operator
**ID:** `JAVA-E1080` | **Lien:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-E1080)

![Major](https://img.shields.io/badge/severity-major-orange) ![Bug Risk](https://img.shields.io/badge/type-bug_risk-green)

This code seems to be multiplying the result of a modulus ( `%` ) operation without specifying the order of operations with parentheses. Such code may not work as expected.


## Bad Practice
Avoid compound expressions where subexpressions are left without parentheses.


```java
assert(i % 60 * 1000 == (i % 60) * 1000) // Succeeds
assert(i % 60 * 1000 == i % (60 * 1000)) // !!! Fails
```

## Recommended
Rewrite the expression to correctly specify order of operations.


```java
i % (60 * 1000)
```
