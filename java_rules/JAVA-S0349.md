# `equals` can't be used to compare arrays of different types
**ID:** `JAVA-S0349` | **Link:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-S0349)

![Major](https://img.shields.io/badge/severity-major-orange)![Bug Risk](https://img.shields.io/badge/type-bug_risk-green)

This method invokes the `equals(Object o)` method to compare two arrays, but the arrays involved are of incompatible types (e.g., `String[]` and `StringBuffer[]`, or `String[]` and `int[]`). They will never be equal. In addition, when `equals(...)` is used to compare arrays it only checks to see if the given references are the same, and ignores the contents of the arrays.

To compare the contents of two arrays, use `java.util.Arrays.equals(Object[], Object[])`.

