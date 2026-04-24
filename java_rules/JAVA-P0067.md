# `Integer`/`Long` constructor is inefficient, use `valueOf` instead
**ID:** `JAVA-P0067` | **Lien:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-P0067)

![Major](https://img.shields.io/badge/severity-major-orange) ![Performance](https://img.shields.io/badge/type-performance-white)

Using `Integer` 's default constructor is guaranteed to always result in a new object whereas `Integer.valueOf` allows the compiler/class library/JVM to cache values, which is known as interning.

Use of cached values avoids object allocation and the resulting code will be faster. Values between -128 and 127 are guaranteed to have corresponding cached instances and using `valueOf` is approximately 3.5 times faster than using the constructor. For values outside the constant range the performance of both styles is the same.


## Bad Practice

```java
Integer a = new Integer(34);
```

## Recommended

```java
Integer a = Integer.valueOf(34);

// or

Integer b = 34; // Autoboxing
```
Unless the class must be compatible with JVMs predating Java 1.5, use either autoboxing or the `valueOf` method when creating instances of `Long` , `Integer` , `Short` , `Character` , and `Byte` .

**Note** - This issue will be ignored within tests.

