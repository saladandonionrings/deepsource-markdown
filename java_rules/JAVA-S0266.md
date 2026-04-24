# Value is guaranteed to be accessed while null after an exception is thrown
**ID:** `JAVA-S0266` | **Link:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-S0266)

![Critical](https://img.shields.io/badge/severity-critical-red)![Bug Risk](https://img.shields.io/badge/type-bug_risk-green)

There is a statement or branch within a catch block which, if executed, guarantees that the variable accessed at this point will be null, unless a runtime exception is thrown (for any reason) before the access.


```java
try {
  // ...
} catch (...) {
  // ...
    value = null; // Value set to null within a catch block
  // ...
}

// possibly within another catch block
value.member += 1; // This is guaranteed to fail with a NPE.
```
Check usages of the concerned value and ensure that any unchecked usages are corrected.

