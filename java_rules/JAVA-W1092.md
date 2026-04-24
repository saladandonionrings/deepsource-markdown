# Catch blocks should be reachable
**ID:** `JAVA-W1092` | **Lien:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-W1092)

![Major](https://img.shields.io/badge/severity-major-orange) ![Anti-pattern](https://img.shields.io/badge/type-anti_pattern-purple)

The Java analyzer has detected an unreachable catch block that is hidden by a preceding catch block.

This will cause an error when the code is compiled.

This issue is raised when a catch block handles exceptions which would already have been handled by a preceding catch block due to the preceding block catching a wider exception type, or the same type.

Consider the following two exception types:


```java
class ParentException extends RuntimeException {
    // ...
}

class ChildException extends ParentException {
    // ...
}
```

## Bad Practice
In the snippet below, the second catch block will never be executed, because the first one will catch all instances of `ChildException` as well.


```java
try {
    // ...
} catch (ParentException a) { // Catches any ChildExceptions as well!!
    // ...
} catch (ChildException b) {
    // ...
}
```

## Recommended
Handle more specific exception types first, then handle parent type exceptions.


```java
try {
    // ...
} catch (ChildException b) { // Catch more specific exceptions first.
    // ...
} catch (ParentException a) {
    // ...
}
```
