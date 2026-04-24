# `Float`/`Double` constructor is inefficient, use `valueOf` instead
**ID:** `JAVA-S0068` | **Lien:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-S0068)

![Major](https://img.shields.io/badge/severity-major-orange) ![Performance](https://img.shields.io/badge/type-performance-white)

Using `Float` or `Double` 's default constructor is guaranteed to always result in a new object whereas the `valueOf` method of these classes allows the JVM to cache values, which is known as interning.

Using cached values avoids object allocation and the resulting code will be faster. Unless the class must be compatible with JVMs predating Java 1.5, use either autoboxing or the `valueOf()` method when creating instances of `Double` and `Float` .


## Examples

## Problematic Code

```java
Float a = new Float(21.422);
```

## Recommended

```java
Float a = 21.422;

// or

Float a = Float.valueOf(21.422);
```
**Note** - This issue will be ignored within tests.

