# Nullable value stored to non-null field
**ID:** `JAVA-E1071` | **Link:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-E1071)

![Critical](https://img.shields.io/badge/severity-critical-red)![Bug Risk](https://img.shields.io/badge/type-bug_risk-green)

A value that could be null is stored into a field that has been annotated as `@Nonnull` or any similar annotation.

This violates any assumptions that consumers of this field may make, and can easily result in crashes and bugs that are harder to diagnose.

Avoid assigning null to any field marked as @Nonnull or any similar annotation.

Add a null check when assigning dynamic values, and do not directly assign null to a non-null annotated variable.


## Bad Practice

```java
@Nonnull
String shouldntBeNull;

// ...

void someMethod() {

    // Avoid assigning null.
    this.shouldntBeNull = null;
}
```

## Recommended
Make sure to null check the result of any nullable expression such as method invocations or variable accesses before assigning to a non-null variable.


```java
String nullableResult = someNullableReturn();

if (nullableResult != null) {
    this.shouldntBeNull = nullableResult;
}
```
