# C style array declaration syntax must not be used
**ID:** `JAVA-C1000` | **Lien:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-C1000)

![Major](https://img.shields.io/badge/severity-major-orange) ![Style](https://img.shields.io/badge/type-style-blue)

Java arrays should not be declared using C-style array declaration syntax.

Declaring arrays using C style syntax is a code smell and reduces the readability of your code. Though this syntax can allow you to write multiple variables whether they are single values or arrays, it can become confusing. In Java, arrays of a type are not the same type as the individual type. Thus, it is conceptually clearer to separate declarations of arrays from declarations of single values of the same type.


## Bad Practice
Avoid declaring arrays in the same line as individual values.


```java
int a = 3, array[] = new int[20];
```

## Recommended
Declare any arrays separately from individual values of the same type.


```java
int a = 3;
int[] array = new int[20];
```
