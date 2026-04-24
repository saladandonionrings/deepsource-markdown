# Primitive values don't need to be compared with `Object.equals()`
**ID:** `JAVA-W1080` | **Lien:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-W1080)

![Major](https://img.shields.io/badge/severity-major-orange) ![Anti-pattern](https://img.shields.io/badge/type-anti_pattern-purple)

Comparing two primitive values with [ `Objects.equals()` ](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/util/Objects.html#equals(java.lang.Object,java.lang.Object)) can be inefficient, since both primitives will have to be boxed before being compared.


## Bad Practice

```java
int someInt = ...;

if (Objects.equals(someInt, 3)) { // unnecessary
    // ...
}
```

## Recommended
Use the `==` operator instead.


```java
if (someInt == 3) {
    // ...
}
```
