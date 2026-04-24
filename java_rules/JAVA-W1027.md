# `toString()` may not work as expected for array types
**ID:** `JAVA-W1027` | **Link:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-W1027)

![Major](https://img.shields.io/badge/severity-major-orange)![Anti-pattern](https://img.shields.io/badge/type-anti_pattern-purple)

This code appears to call `toString()` on an array type (like `String[]`). This will only print out the address of this array in the Java heap, and will not print its contents at all.

Consider using [`Arrays.toString(Object[])`](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/util/Arrays.html#toString(java.lang.Object%5B%5D)) or [`Arrays.deepToString(Object[])`](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/util/Arrays.html#deepToString(java.lang.Object%5B%5D)) to convert the array into a string instead.

Java's native array types simply inherit their implementation of methods such as `toString()`, `equals(Object)` and `hashCode()` from `java.lang.Object`. Thus, calling these methods directly is generally not useful for most purposes.

You can use [`java.util.Arrays.toString(Object[])`](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/util/Arrays.html#toString(java.lang.Object%5B%5D)) or [`java.util.Arrays.deepToString(Object[])`](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/util/Arrays.html#deepToString(java.lang.Object%5B%5D)) (if you want to recursively stringify a nested array) instead to properly take the array's elements into account.


## Bad Practice

```java
String[] strs = ...;

System.out.println(strs.toString()); // Doesn't print the whole array!
```

## Recommended

```java
System.out.println(Arrays.toString(strs));
```
