# `ZoneId.of("Z")` should be replaced with `ZoneOffset.UTC`
**ID:** `JAVA-W1085` | **Lien:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-W1085)

![Major](https://img.shields.io/badge/severity-major-orange) ![Anti-pattern](https://img.shields.io/badge/type-anti_pattern-purple)

Avoid calling `ZoneId.of()` to get the UTC timezone offset, and instead use `ZoneOffset.UTC` directly.


## Bad Practice

```java
ZoneId utc = ZoneId.of("Z");
```

## Recommended

```java
ZoneId utc = ZoneOffset.UTC;
```
