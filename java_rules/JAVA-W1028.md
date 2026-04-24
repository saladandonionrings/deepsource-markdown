# `equals()` method parameters should not be marked with `@NotNull` or equivalent annotations
**ID:** `JAVA-W1028` | **Link:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-W1028)

![Major](https://img.shields.io/badge/severity-major-orange)![Anti-pattern](https://img.shields.io/badge/type-anti_pattern-purple)

Implementations of the `equals()` method should not indicate to API consumers that they expect their argument to be non-null.

API consumers should leave null checking to the `equals()` implementation.

According to the standard library documentation for [`java.lang.Object.equals()`](https://docs.oracle.com/javase/7/docs/api/java/lang/Object.html#equals(java.lang.Object)), the `equals` method should also account for nullable values. This means that the argument to `equals()` cannot be assumed to be non-null and must be checked before comparison within the method.

When a null value is encountered, `equals` is expected to return `false`. Making the API consumer perform the null check before performing an equality check will result in code bloat.

Even if the method does not expect null values, Java's standard library APIs, as well as conforming third party APIs may still pass it null values, possibly crashing the application.


## Bad Practice

```java
static class SomeClass {
    int r = 23;

    @Override
    public boolean equals(@Nonnull Object other) {
        // Regardless of whether a null check occurs in the implementation,
        // the API of equals should not inform users that it expects non-null values.
        if (!(other instanceof SomeClass)) return false;
        return r == ((SomeClass) other).r;
    }
}
```

## Recommended
Remove the annotation.


```java
@Override
    public boolean equals(Object other) {
        if (!(other instanceof SomeClass)) return false;
        return r == ((SomeClass) other).r;
    }
```
