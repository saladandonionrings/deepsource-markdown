# Avoid using a single unescaped `.` as a regex pattern
**ID:** `JAVA-W1078` | **Lien:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-W1078)

![Major](https://img.shields.io/badge/severity-major-orange) ![Anti-pattern](https://img.shields.io/badge/type-anti_pattern-purple)

A single unescaped `.` in a regex pattern will match one of any character, not just the period character.

Consider escaping the `.` if you meant to match a single period instead.


## Bad Practice
This regex matches anything but newlines, once:


```java
Pattern p = Pattern.compile(".");
```

## Recommended
Escape the pattern with two `\` characters if you are matching a single period.


```java
Pattern p = Pattern.compile("\\.");
```
