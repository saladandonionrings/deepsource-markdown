# Local variable is never written to
**ID:** `JAVA-E1064` | **Lien:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-E1064)

![Critical](https://img.shields.io/badge/severity-critical-red) ![Bug Risk](https://img.shields.io/badge/type-bug_risk-green)

A local variable of a method or constructor has been found to have no writes to it.

This will cause a compile error when the project is built.

Java always expects a local variable to be assigned a value before it is read from. If no assignment is performed, Java will raise a compile error indicating that the variable was expected to be initialized before use.


## Bad Practice
In the example below, `x` is not written to before use.


```java
int someMethod() {
    int x;

    // x is never written to.

    return x + 5; // will cause a compiler error.
}
```

## Recommended
Always initialize variables to a sensible default value before using them.


```java
int someMethod() {
    int x = 0;

    return x + 5;
}
```
