# Use `existsById` instead of `findById` to check for the existence of an entity
**ID:** `JAVA-W1090` | **Link:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-W1090)

![Major](https://img.shields.io/badge/severity-major-orange)![Anti-pattern](https://img.shields.io/badge/type-anti_pattern-purple)

Use `existsById()` instead of `findById()` if you are calling `findById()` for the sole purpose of checking for the existence of an entity in the repository.

This issue is raised when the Java analyzer detects a usage of `findById()` where the returned value is only used in a null check.


## Bad Practice

```java
if (repository.findById(idValue) != null) {
    // do some operation with the id.
}
```

## Recommended
Use the `existsById` method to check for the presence of an entity in the repository instead.


```java
if (repository.existsById(idValue)) {
    // do some operation with the id.
}
```
If your repository definition has no `existsById` method, consider adding it in.


```java
// In the repository interface
    boolean existsById(<your ID type here> id);
```
