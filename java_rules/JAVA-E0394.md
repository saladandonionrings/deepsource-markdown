# Invalid regex syntax must not be used
**ID:** `JAVA-E0394` | **Lien:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-E0394)

![Critical](https://img.shields.io/badge/severity-critical-red) ![Bug Risk](https://img.shields.io/badge/type-bug_risk-green)

Invalid regex strings will throw a `PatternSyntaxException` at runtime.

This code attempts to compile a regular expression string that is invalid according to Java's syntax for regular expressions. Because of this, a `PatternSyntaxException` will be thrown when this statement is executed.


## Bad Practice

```java
Pattern.compile("(\\)");
```

## Recommended
Perhaps the intention was to match on a single `\\` character:


```java
Pattern.compile("(\\\\)");
```
