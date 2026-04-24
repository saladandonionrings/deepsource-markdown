# Inefficient use of `String` constructor
**ID:** `JAVA-S0062` | **Lien:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-S0062)

![Major](https://img.shields.io/badge/severity-major-orange) ![Performance](https://img.shields.io/badge/type-performance-white)

Creating a `String` using object creation wastes memory because the new `String` object so constructed will be functionally indistinguishable from the `String` value passed as a parameter. Just use the string directly.


## Examples

## Problematic Code

```java
String a = new String("abc");
```

## Recommended

```java
String a = "abc";
```
