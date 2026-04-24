# Negating the result of `compareTo`/`compare` may have unexpected results
**ID:** `JAVA-W1048` | **Lien:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-W1048)

![Major](https://img.shields.io/badge/severity-major-orange) ![Anti-pattern](https://img.shields.io/badge/type-anti_pattern-purple)

This code negates the return value of a `compareTo` or `compare` method. Avoid doing so, as it is both confusing and may lead to unexpected behavior.

This is a questionable or bad programming practice, since if the return value is `Integer.MIN_VALUE` , negating the return value won't negate the sign of the result. Additionally, such code may confuse anybody who reads it.


## Bad Practice
Avoid negating the result of `compareTo` or any `compare` method.


```java
if (-a.compareTo(b) > 0) {
    // ...
}
```

## Recommended
Perform the intended operation directly, either by changing the operator, or reversing the order of the operands.


```java
if (a.compareTo(b) < 0) {
    // ...
}
```
