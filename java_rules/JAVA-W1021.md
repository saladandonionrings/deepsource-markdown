# Redundant type check
**ID:** `JAVA-W1021` | **Link:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-W1021)

![Major](https://img.shields.io/badge/severity-major-orange)![Anti-pattern](https://img.shields.io/badge/type-anti_pattern-purple)

This code attempts to perform an `instanceof` check of a type with its supertype. This is a redundant operation as such a check will always return `true`.

For two classes `A` and `B`, where `A extends B`, an instance of `A` can also be treated as an instance of `B`. Thus, a check such as `<some instance of A> isntanceof B` will always return `true`.

This may have been a typo. Check if the type you are checking for is correct, and rectify it if not.


## Bad Practice

```java
Integer a = 3;

// `a` is already a Number
if (a instanceof Number) {
    // ...
}
```

## Recommended
If the check serves no purpose, remove it. Else, verify that the proper type is being checked for.

