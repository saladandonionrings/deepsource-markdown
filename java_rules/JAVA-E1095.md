# Methods annotated as non-nullable should not return null values
**ID:** `JAVA-E1095` | **Lien:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-E1095)

![Critical](https://img.shields.io/badge/severity-critical-red) ![Bug Risk](https://img.shields.io/badge/type-bug_risk-green)

A method that is marked with annotations such as `@Nonnull` should not return explicit null values.

Returning null when the method is explicitly marked as non-null is a bad practice, since it defeats the purpose of the method being annotated in the first place.


## Bad Practice

```java
@Nonnull
String someMethod() {
    // ...

    if (someCondition) {
        return null; // Not right!
    }

    // ...
    return value;
}
```

## Recommended
Avoid returning null if the method is marked as not returning null.

