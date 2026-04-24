# Sealed class/interface permitted types need not be listed if they are declared in the same file
**ID:** `JAVA-W1031` | **Link:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-W1031)

![Major](https://img.shields.io/badge/severity-major-orange)![Anti-pattern](https://img.shields.io/badge/type-anti_pattern-purple)

A sealed class/interface does not need to declare a `permits` clause if all its subtypes are declared within the same file as the sealed type.

Sealed classes were recently stabilised in Java 17, and provide a way to control whether users can create new subclasses from a particular class. The Java Language Specification [Section 8.1.1.2](https://docs.oracle.com/javase/specs/jls/se17/html/jls-8.html#jls-8.1.1.2) mentions that a sealed class need not have a `permits` clause if all direct child classes are placed in the same file.


## Bad Practice

```java
// The permits clause isn't required!
sealed class Sealed permits Child1, Child2 {
    // ...
}

final class Child1 extends Sealed {}
final class Child2 extends Sealed {}
```

## Recommended
Remove the permits clause.


```java
sealed class Sealed {
    // ...
}

final class Child1 extends Sealed {}
final class Child2 extends Sealed {}
```
