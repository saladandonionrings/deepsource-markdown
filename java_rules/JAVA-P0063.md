# Use `""` instead of `new String()` to create empty strings
**ID:** `JAVA-P0063` | **Lien:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-P0063)

![Major](https://img.shields.io/badge/severity-major-orange) ![Performance](https://img.shields.io/badge/type-performance-white)

Creating a new `java.lang.String` object using the default constructor wastes memory because the object so created will be functionally indistinguishable from the empty string constant `""` .

Java guarantees that identical string constants will be represented by the same `String` object. Therefore, you should just use the empty string constant directly.


## Bad Practice

```java
String a = new String("");
```

## Recommended

```java
String a = "";
```
