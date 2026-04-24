# Protected fields in a final class are useless
**ID:** `JAVA-W0417` | **Lien:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-W0417)

![Minor](https://img.shields.io/badge/severity-minor-yellow) ![Anti-pattern](https://img.shields.io/badge/type-anti_pattern-purple)

This class is declared to be final, but declares fields to be protected. Such code is confusing, since protected fields in final classes are effectively the same as private fields.


## Bad Practice

```java
final class A {

      // This field is effectively private since A cannot be subclassed further.
      protected int abc;

}
```

## Recommended
Since the class is final, it can not be derived from, and a protected field in a final class is essentially private. The access modifier for the field should be changed to `private` or `public` to represent the true use for the field.


```java
final class A {

      private int abc;

}
```
