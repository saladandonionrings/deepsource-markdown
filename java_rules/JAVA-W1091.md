# Use `assertNull`/`NotNull` instead of `assertEquals`/`notEquals` to assert nullity
**ID:** `JAVA-W1091` | **Link:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-W1091)

![Major](https://img.shields.io/badge/severity-major-orange)![Anti-pattern](https://img.shields.io/badge/type-anti_pattern-purple)

Use the `assertNull` and `assertNotNull` methods instead of using `assertEquals` or `assertNotEquals` with an expected `null` argument.

This issue is raised when the Java analyzer detects a usage of `assertEquals` or `assertNotEquals` where one of the arguments is a null value.


## Bad Practice

```java
Assertions.assertEquals(null, result);
```

## Recommended
Use the `assertNull` and `assertNotNull` methods instead:


```java
Assertions.assertNull(result);
```
