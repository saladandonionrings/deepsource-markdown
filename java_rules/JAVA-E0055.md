# Fields of immutable classes should be final
**ID:** `JAVA-E0055` | **Link:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-E0055)

![Major](https://img.shields.io/badge/severity-major-orange)![Bug Risk](https://img.shields.io/badge/type-bug_risk-green)

The class is annotated with `net.jcip.annotations.Immutable` or `javax.annotation.concurrent.Immutable`, and the rules for those annotations require that all public fields are final.

Any code that relies on such assumptions may fail unexpectedly if any mutation of a value of this class occurs.

While internal mutability is ok in some cases to optimize the implementation of the class, such mutable state should not be externally visible in any way. State introduced into the class (e.g. by passing in some object value) must be cloned to ensure that internal state is not affected by external factors. Similarly, care must be taken to ensure that any exposed fields cannot be mutated externally either.


## Bad Practice

```java
@Immutable
class A {
    public Object field1; // This should be final.

    // ...
}
```

## Recommended

```java
@Immutable
class A {
    public final Object field1;

}
```
