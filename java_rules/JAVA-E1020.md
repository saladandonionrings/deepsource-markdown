# Indices must not be out of bounds
**ID:** `JAVA-E1020` | **Link:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-E1020)

![Critical](https://img.shields.io/badge/severity-critical-red)![Bug Risk](https://img.shields.io/badge/type-bug_risk-green)

When indexing into a `String`, `List` or array, if the index is out of bounds one of the `IndexOutOfBoundsException` family will be thrown.

This issue will be raised if the indices used with `String`, `List` or array access expressions appear to be out of bounds with respect to the target value's declared size.


## Bad Practice

```java
String s = "abcde";

Boolean[] b = new Boolean[234];

// These access statements all pass in invalid indices.
String e = s.subSequence(1, -23);
e = s.substring(34);
char c = s.charAt(7);

// Bad array access expressions!
b[-1];
b[756];
```

## Recommended
Ensure that array/list accesses agree with the maximum capacity of the underlying list at the time of use.


```java
String s = "abcde";

String e = s.subSequence(1, 4);
e = s.substring(2);
char c = s.charAt(3);
```
