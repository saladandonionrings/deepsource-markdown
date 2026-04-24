# Calling String.indexOf() with a single character string is inefficient
**ID:** `JAVA-P1004` | **Lien:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-P1004)

![Major](https://img.shields.io/badge/severity-major-orange) ![Performance](https://img.shields.io/badge/type-performance-white)

This code passes a single character string, or an empty string to [ `String.indexOf()` ](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/lang/String.html#indexOf(java.lang.String)) . Doing so is useless at best, and is also more inefficient than passing a character directly.


## Bad Practice

```java
// Use a character or int value instead.
"abc.cat".indexOf(".");

// This will always return index 0.
"abc.cat".indexOf("");
```
If there is an empty string in the first argument to `indexOf` , it may indicate that a typo was committed.


## Recommended
It is more efficient to use the integer implementations of [ `indexOf()` ](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/lang/String.html#indexOf(int)) :


```java
myString.indexOf('.')
```
