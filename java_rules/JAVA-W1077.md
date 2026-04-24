# `String.substring()` call with single `0` index found
**ID:** `JAVA-W1077` | **Lien:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-W1077)

![Major](https://img.shields.io/badge/severity-major-orange) ![Anti-pattern](https://img.shields.io/badge/type-anti_pattern-purple)

Avoid calling [ `String.substring(int)` ](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/lang/String.html#substring(int)) with just `0` , as that would effectively just return a copy of the string.

`String` s are immutable, and it's possible to copy a string just with its reference. If you absolutely need a new string (for referential identity purposes, maybe), use `String` 's copy constructor.


## Bad Practice

```java
String a = "something";
String b = a.substring(0); // not very useful...
```

## Recommended
To actually copy a `String` , you can construct a new one with `String` 's [copy constructor](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/lang/String.html#%3Cinit%3E(java.lang.String)) :


```java
String b = new String(a);
```
