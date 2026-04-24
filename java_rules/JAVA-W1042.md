# Overly generic exceptions should not be thrown
**ID:** `JAVA-W1042` | **Lien:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-W1042)

![Major](https://img.shields.io/badge/severity-major-orange) ![Anti-pattern](https://img.shields.io/badge/type-anti_pattern-purple)

`throws` clauses should not contain generic exception types such as `Throwable` , `Exception` , or `RuntimeException` .

Instead, extend `RuntimeException` and create more specific exception types which are relevant to your use case.

This issue will be reported for method and constructor declarations with `throws` clauses that contain any of the following exception types:

* `java.lang.Throwable`
* `java.lang.Exception`
* `java.lang.RuntimeException`


## Bad Practice
Avoid using overly generic exception types:


```java
public float getPercent() throws RuntimeException { ... }
```

## Recommended
Use a more specific exception type instead.


```java
class CalculationException extends RuntimeException {
    // ...
}

// ...

public float getPercent() throws CalculationException { ... }
```
