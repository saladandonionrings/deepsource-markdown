# `@Expensive`/`@WorkerThread` annotated method should not override unannotated super method
**ID:** `JAVA-W1061` | **Lien:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-W1061)

![Major](https://img.shields.io/badge/severity-major-orange) ![Anti-pattern](https://img.shields.io/badge/type-anti_pattern-purple)

An overriding method should not be marked `@Expensive` or `@WorkerThread` if its super method is not annotated as such.

It's recommended to bump all such annotations to the interface or the super class method declaration that the outside world will be interacting with.

In Java, it is encouraged to work with abstract interfaces and/or classes. If some implementation of a method may turn out to be expensive, users of the method must be aware of that. Annotating subtype methods has the nasty implication that the 3rd party code which uses the abstract api will have no way of knowing if a call to that method turns out to be expensive until it is benchmarked. This is a contract violation; a method that behaves a certain way doesn't fully specify the behavior at the API boundary.


## Bad Practice

```java
interface Service {
    public void maybeExpensive();
}

class Subtype implements Service {
    @Override
    @Expensive
    public void maybeExpensive() {
        // ...some expensive job
    }
}
```

## Recommended
Consider annotating supertype methods.


```java
interface Service {
    @Expensive
    public void maybeExpensive();
}

class Subtype implements Service {
    @Override
    @Expensive
    public void maybeExpensive() {
        // ...some expensive job
    }
}
```
