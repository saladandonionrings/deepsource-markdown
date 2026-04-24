# `@Deprecated` should not be applied to local variables or parameters
**ID:** `JAVA-W1082` | **Lien:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-W1082)

![Major](https://img.shields.io/badge/severity-major-orange) ![Anti-pattern](https://img.shields.io/badge/type-anti_pattern-purple)

Avoid marking parameters or local variables as `@Deprecated` , as the annotation will have no effect.


## Bad Practice
In the method below, the argument `input` is marked with `@Deprecated` . However, this annotation will have no semantic meaning, and `javac` will not generate a deprecation warning for it like it would for usage of a method annotated with `@Deprecated` .


```java
public static String getSomething(@Deprecated String input) {
    // ...
}
```

## Recommended
Avoid marking parameters and local variables as `@Deprecated` .

