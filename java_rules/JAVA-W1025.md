# Unused private field detected
**ID:** `JAVA-W1025` | **Lien:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-W1025)

![Major](https://img.shields.io/badge/severity-major-orange) ![Anti-pattern](https://img.shields.io/badge/type-anti_pattern-purple)

A private field which is not referenced anywhere in this file was detected.

Such a field is useless and can be safely removed.


## Bad Practice

```java
class SomeClass {
    private int unused; // Not used anywhere within `SomeClass`.

    // ...
}
```

## Recommended
Remove the field if is is not used anywhere. If the field was meant to be inherited, mark it as `protected` instead.


```java
class SomeClass {
    protected int usedInSubclass;

    // ...
}
```
