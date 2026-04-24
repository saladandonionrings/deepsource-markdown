# Arrays are not equal to non-array values
**ID:** `JAVA-S0347` | **Link:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-S0347)

![Major](https://img.shields.io/badge/severity-major-orange)![Bug Risk](https://img.shields.io/badge/type-bug_risk-green)

This method invokes the `equals(Object o)` method to compare an array and a reference that doesn't seem to be an array. If the objects being compared are of different types, they are guaranteed to be unequal and the comparison is almost certainly an error. Even if they are both arrays, the `equals` method on arrays only determines if the two arrays are the same object.

To compare the contents of two arrays, use `java.util.Arrays.equals(Object[], Object[])`.

