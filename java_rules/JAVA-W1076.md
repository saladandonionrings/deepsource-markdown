# Avoid catching assertions in tests
**ID:** `JAVA-W1076` | **Lien:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-W1076)

![Critical](https://img.shields.io/badge/severity-critical-red) ![Bug Risk](https://img.shields.io/badge/type-bug_risk-green)

Avoid catching assertion exceptions, they are meant to indicate that something should not have happened.

This can happen when there is a catch block for `Throwable` or `Error` , or if one tries to directly catch an `AssertionError` or its descendents.

Because `Throwable` is the parent of all exception and error types, and even assertion failures are represented as thrown `Error` s, it is inadvisable to catch either of `Throwable` or `Error` in a test.


## Bad Practice

```java
try {
    assertTrue(someCondition);
} catch (Throwable e) { // Don't catch Throwable in tests!
    // ...
}
```

## Recommended
Avoid catching overly generic exception types such as `Throwable` or `Error` , and do not attempt to catch `AssertionError` s.

If you really require generic exception handling within a test, catch only `Exception` s.


```java
try {
    assertTrue(someCondition);
} catch (Exception e) {  // this will not interfere with assertions.
    // ...
}
```
