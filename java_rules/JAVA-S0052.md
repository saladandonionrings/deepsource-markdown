# Method may ignore exceptions
**ID:** `JAVA-S0052` | **Lien:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-S0052)

![Major](https://img.shields.io/badge/severity-major-orange) ![Anti-pattern](https://img.shields.io/badge/type-anti_pattern-purple)

This method might ignore an exception. In general, exceptions should be handled, reported in some way, or rethrown by the method.

Not handling exceptions properly may result in bugs that go unnoticed until it is too late.


## Examples

## Problematic Code

```java
try {
    // ...
} catch (NoSuchElementException e) {
    // ...
} catch(Exception e) {
    // Nothing here
}
```

## Recommended
Consider at least logging the exception to ensure that issues that may actually be bugs are not missed.


```java
try {
    // ...
} catch (NoSuchElementException e) {
    // ...
} catch(Exception e) {
    System.err.println(e.message);
}
```
