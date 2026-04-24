# Boxed primitives will be unboxed and coerced to a common type in ternary operation
**ID:** `JAVA-W1055` | **Link:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-W1055)

![Major](https://img.shields.io/badge/severity-major-orange)![Anti-pattern](https://img.shields.io/badge/type-anti_pattern-purple)

A boxed primitive is unboxed and converted to another boxed type as part of the evaluation of a conditional ternary operator (like in `b ? e1 : e2`).


## Bad Practice

```java
Integer e1 = 3;
Float e2 = 2.0f;

// e1 will be unboxed and coerced to Float here.
Float c = 3 > 2 ? e1 : e2;
```
The semantics of Java mandate that if `e1` and `e2` are boxed primitives, the values are unboxed and converted/coerced to their common type (e.g, if `e1` is an `Integer` and `e2` is a `Float`, then `e1` is unboxed, converted to a `Float`, and boxed again.


## Recommended
The best way to fix this is to avoid mixing values of different boxed types within ternary expressions.


```java
Float e1 = 3f;
Float e2 = 2f;

Float c = 3 > 2 ? e1 : e2;
```

## Exceptions
This issue will be ignored in tests.

