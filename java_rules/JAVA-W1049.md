# Needless boxing of a primitive only to call `toString`
**ID:** `JAVA-W1049` | **Link:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-W1049)

![Minor](https://img.shields.io/badge/severity-minor-yellow)![Performance](https://img.shields.io/badge/type-performance-white)

A boxed primitive is allocated just to call `toString()`. It is more effective to just use the static form of `toString()` which takes the primitive value.


## Bad Practice
Expressions such as the one below are redundant.


```java
Float.valueOf(1.0f).toString()
```

## Recommended
Use the static `toString()` method of the respective wrapper type, or [`String.valueOf()`](https://docs.oracle.com/javase/8/docs/api/java/lang/String.html) instead.


```java
Float.toString(1.0f)

// OR

String.valueOf(1.0f);
```
