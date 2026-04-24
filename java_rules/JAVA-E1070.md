# NullPointerException should not be caught
**ID:** `JAVA-E1070` | **Link:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-E1070)

![Critical](https://img.shields.io/badge/severity-critical-red)![Bug Risk](https://img.shields.io/badge/type-bug_risk-green)

This code appears to catch a `NullPointerException`. This may hide bad errors in code.

Consider removing the offending clause and debugging the underlying cause of the exception instead.

When an NPE is caught solely so it can be silenced, it can indicate that the underlying cause of the exception is not properly known. If an NPE is thrown, it may be that the application is in an inconsistent state which should not be ignored.


## Bad Practice

```java
try {
    // ...
} catch (NullPointerException n) { // Debug the cause instead!
    n.printStackTrace();
}
```

## Recommended
Remove the catch clause that handles `NullPointerException` and debug the underlying issue instead.


## Exceptions
If the reason for throwing NPEs is within library code or within code that you have no control over, it may not be possible to easily fix the issue. In such cases, consider ignoring this issue with a `skipcq` comment above the offending line.


```java
// skipcq
    somethingThatThrows();
```
