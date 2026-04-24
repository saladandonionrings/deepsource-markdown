# Class is not an Exception/Throwable, even though it is named as such
**ID:** `JAVA-W0182` | **Link:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-W0182)

![Minor](https://img.shields.io/badge/severity-minor-yellow)![Anti-pattern](https://img.shields.io/badge/type-anti_pattern-purple)

This class is not an exception, and does not extend `Throwable` or any other exception class, but ends with `'Exception'`. This may be confusing to users of this class.


## Bad Practice

```java
class HandleException {
    // ... some code to do with handling exceptions?
}
```

## Recommended
Consider renaming the class to be less confusing:


```java
class ExceptionHandler {
    // ...
}
```
