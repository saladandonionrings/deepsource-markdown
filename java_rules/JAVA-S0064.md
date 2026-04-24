# `toString` invoked on a string value is useless
**ID:** `JAVA-S0064` | **Lien:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-S0064)

![Major](https://img.shields.io/badge/severity-major-orange) ![Performance](https://img.shields.io/badge/type-performance-white)

Calling `String.toString` is a redundant operation. Just use the string directly.


## Examples

## Problematic Code

```java
String b = "abc".toString();
```

## Recommended

```java
String b = "abc";
```

## Exceptions
There are some exceptions to this, such as within generated code where such statements are likely to appear. Consider adding these files to the `exclude_files` or `exclude_patterns` to reduce false positives.

