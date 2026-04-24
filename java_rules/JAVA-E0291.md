# Self assignment of local variable detected
**ID:** `JAVA-E0291` | **Link:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-E0291)

![Critical](https://img.shields.io/badge/severity-critical-red)![Bug Risk](https://img.shields.io/badge/type-bug_risk-green)

A local variable is assigned to itself.

This is essentially a noop but it may be indicative of a different problem. It may be that the variable shadows another in a parent scope, or that the variable may shadow a field of the object itself. Such code can cause confusion and subtle logic errors that are hard to catch.


## Bad Practice

```java
public void foo() {
    int x = 3;
    int y = someInt;
    // ...
    x = x; // Useless self assignment.
}
```

## Recommended
Always check that the correct variable is being assigned to (or from)


```java
public void foo() {
    int x = 3;
    int y = someInt;
    // ...
    y = x; // Here, we assign x to y.
}
```
Check if you meant to assign a field or another local variable with a similar name instead.

