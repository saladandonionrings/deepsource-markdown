# Type names must begin with an uppercase letter
**ID:** `JAVA-C1001` | **Link:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-C1001)

![Major](https://img.shields.io/badge/severity-major-orange)![Style](https://img.shields.io/badge/type-style-blue)

Java naming conventions must be followed for all type names.

As per the conventions, all type (class, enum, interface) names in Java must begin with an uppercase letter. Not following this convention would make it harder to search for such type declarations in large source files.


## Bad Practice
Avoid typenames that start with a lowercase letter.


```java
class klass {
    //..rest of the code
}
```

## Recommended
Consider following the conventions when naming your types.


```java
class Klass {
    //..rest of the code
}
```
