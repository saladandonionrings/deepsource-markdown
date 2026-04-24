# Returned `Future`s should not be ignored
**ID:** `JAVA-W1087` | **Lien:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-W1087)

![Major](https://img.shields.io/badge/severity-major-orange) ![Anti-pattern](https://img.shields.io/badge/type-anti_pattern-purple)

Always use the value returned by a method with return type `Future<T>` .

When a method returns a `Future` , it means the result of its computation will only be available at a later time, not immediately. It is possible that the operation may fail with an exception, or may have some useful result.

If the return value is ignored, such data will be lost.

If, however, there truly is no reason to use the result of the future, consider marking this call with a `// skipcq: JAVA-W1087` comment to avoid reporting this issue. This issue will also respect suppression via `SuppressWarnings("unused")` .


## Bad Practice

```java
CompletableFuture<String> returnsFuture() {
    return someImportantDataInAFuture;
}

void consumer() {
    returnsFuture(); // Bad!
}
```

## Recommended
Even if there is no use for the data, consider adding a handler to process any exceptions that may have occurred during the future's execution.


```java
returnsFuture().handle((v, e) -> {
    if (e != null) 
        logger.error("An error occurred.", e);
});
```
