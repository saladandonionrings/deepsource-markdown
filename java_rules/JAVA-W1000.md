# Exception classes must be named appropriately
**ID:** `JAVA-W1000` | **Lien:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-W1000)

![Minor](https://img.shields.io/badge/severity-minor-yellow) ![Anti-pattern](https://img.shields.io/badge/type-anti_pattern-purple)

This class is an exception, but its name does not end in `Exception` . This could be confusing to consumers of your API.


## Bad Practice

```java
class BadName extends Exception {
    // ...
}
```

## Recommended

```java
class ActuallyAnException extends Exception {
    // ...
}
```
