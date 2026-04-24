# Format strings should use `%n` instead of `\\n`
**ID:** `JAVA-W0379` | **Link:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-W0379)

![Minor](https://img.shields.io/badge/severity-minor-yellow)![Anti-pattern](https://img.shields.io/badge/type-anti_pattern-purple)

This format string includes a newline character (`\\n`). This may cause issues on platforms like Windows that do not use Unix line separators.

In format strings, it is generally preferable to use `%n`, which will produce the platform-specific line separator.


## Bad practice

```java
String.format("%s\\n%d", "number", 3);
```

## Recommended

```java
String.format("%s%n%d", "number", 3);
```
