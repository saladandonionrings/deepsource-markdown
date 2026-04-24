# Empty catch clauses may hide exceptions
**ID:** `JAVA-E0052` | **Link:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-E0052)

![Major](https://img.shields.io/badge/severity-major-orange)![Anti-pattern](https://img.shields.io/badge/type-anti_pattern-purple)

When a `catch` clause is empty, it essentially ignores any occurrences of the particular exception it handles. This could allow critical bugs to go undiagnosed because any relevant exceptions indicative of a bug would be discarded within this `catch` block.


## Examples

## Problematic Code

```java
try {
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
} catch(Exception e) {
    System.err.println(e.message); // It may be better to make use of a more robust logging solution like logback.
}
```
