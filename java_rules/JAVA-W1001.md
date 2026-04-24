# File path does not match declared package
**ID:** `JAVA-W1001` | **Lien:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-W1001)

![Major](https://img.shields.io/badge/severity-major-orange) ![Anti-pattern](https://img.shields.io/badge/type-anti_pattern-purple)

This file's path does not match its declared package. While the Java compiler will not complain about this, it is a patently bad idea to do so. Change the package declaration to match the file system path.

Java does not care where class files that are to be compiled exist by default, but this does not mean developers shouldn't.


## Bad Practice
In `./src/example/Main.java` :


```java
package com.example;

// ...
```
This file declares its package to be `com.example` , despite the fact that its path relative to the source directory indicates that the package is `example` . The difference in the declared and assumed paths could lead to errors when importing classes defined in this file, and will likely confuse readers of this file.


## Recommended
Package directives must always match the source file's path relative to the source root directory.


```java
package example;
```
