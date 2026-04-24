# Abstract class constructors should not be public
**ID:** `JAVA-W1094` | **Lien:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-W1094)

![Major](https://img.shields.io/badge/severity-major-orange) ![Anti-pattern](https://img.shields.io/badge/type-anti_pattern-purple)

Abstract classes cannot be instantiated, so their constructors need not be public. Consider marking the constructor as protected instead.


## Bad Practice

```java
abstract class SomeClass {
    public SomeClass(...) {
        // ...
    }
}
```

## Recommended
Make the constructor protected.


```java
abstract class SomeClass {
    protected SomeClass(...) {
        // ...
    }
}
```
