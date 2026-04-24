# `Boolean` constructor is inefficient, consider using `Boolean.valueOf` instead
**ID:** `JAVA-P0066` | **Lien:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-P0066)

![Major](https://img.shields.io/badge/severity-major-orange) ![Performance](https://img.shields.io/badge/type-performance-white)

Creating new instances of `java.lang.Boolean` wastes memory, since `Boolean` objects are immutable and there are only two useful values of this type.


## Bad Practice

```java
Boolean a = new Boolean(true);
```

## Recommended
Use the `Boolean.valueOf` method (or autoboxing since Java 1.5) to create `Boolean` objects instead.


```java
Boolean a = true;

// or

Boolean b = Boolean.valueOf(true);
```
**Note** - This issue will be ignored within tests.

