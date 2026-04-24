# Use of `@Nonnull`, `@CheckForNull`, or `@Nullable` detected on primitive declaration
**ID:** `JAVA-W1063` | **Lien:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-W1063)

![Major](https://img.shields.io/badge/severity-major-orange) ![Anti-pattern](https://img.shields.io/badge/type-anti_pattern-purple)

Primitive types can't be `null` . Marking primitive parameters, return values, or fields with `CheckForNull` , `Nullable` , or `NonNull` is useless and only adds confusion. These annotations should be removed to improve readability of code.


## Bad Practice

```java
class Example {
    @Nullable private int field = 10;

    @CheckForNull
    public int method(@NonNull int param) {
        // ...some code
    }
}
```

## Recommended
Remove these annotations from primitive declarations.


```java
class Example {
    private int field = 10;

    public int method(int param) {
        // ...some code
    }
}
}
```
