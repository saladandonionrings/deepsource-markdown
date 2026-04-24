# Unnecessary imports detected
**ID:** `JAVA-W1069` | **Link:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-W1069)

![Major](https://img.shields.io/badge/severity-major-orange)![Anti-pattern](https://img.shields.io/badge/type-anti_pattern-purple)

Unused imports should be removed from all source files.

If a type is never used, there is no point in importing it in a class. Such imports should be removed from source files. This helps readability and also prevents potential name clashes in the source file.

This issue will be reported when no usages, including doc comments, can be seen for a particular import.


## Bad Practice

```java
import java.io.File;

// ...rest of the source that never uses `java.io.File`
```

## Recommended
Just remove unused imports. Most modern IDEs provide handy shortcuts for that. If that's not an option for you, consider removing them manually.

