# Downcast may flip integer sign in comparator method
**ID:** `JAVA-E1102` | **Link:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-E1102)

![Major](https://img.shields.io/badge/severity-major-orange)![Bug Risk](https://img.shields.io/badge/type-bug_risk-green)

The Java analyzer has detected a narrowing cast of a subtraction in a comparison method that may flip the sign of the result.

Methods such as [`compare`](https://docs.oracle.com/javase/8/docs/api/java/util/Comparator.html#compare-T-T-) (from `Comparator`) and [`compareTo`](https://docs.oracle.com/javase/8/docs/api/index.html?java/util/Comparator.html) (from `Comparable`) are used to check properties such as which of two values is greater or less than the other.

While subtracting two variables usually is a good way to compare two integral values, it is a bad idea to downcast the result to a smaller type.


## Bad Practice
Consider this case of two numbers being subtracted:


```java
int a = 0x0000F09;
int b = 0x0FFFFD30;

int c = a - b;    // 0xF00011D9
int d = (short)c; // 0x000011D9 !!!
```
`d` is completely different from `c` because the 4 most significant bits of `c` (`F`) have been removed due to the downcast from `int` to `short`. This converts the value from a negative 2's complement value to a positive one.


## Recommended
Use `Long.compare()` to compare integers of any size instead. this method will return a correct, usable comparison result in all cases.


```java
return Long.compare(a, b);
```
