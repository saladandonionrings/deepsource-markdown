# Multiple `@Inject` constructors found
**ID:** `JAVA-W1074` | **Link:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-W1074)

![Major](https://img.shields.io/badge/severity-major-orange)![Anti-pattern](https://img.shields.io/badge/type-anti_pattern-purple)

There should only be one constructor that is annotated with `@Inject` in a class.

`@Inject` annotation is used to indicate that a particular constructor will be used by some 3rd-party code (e.g.: a framework) to construct instances of a class. In the presence of multiple `@Inject` constructors, frameworks may not reliably choose a constructor. Furthermore, different versions of the same framework may end up choosing different constructors leading to different runtime behavior of the program.


## Bad Practice

```java
import javax.inject.Inject;

public class Klass {
    @Inject
    public Klass() {
        // First `@Inject` constructor.
    }

    @Inject
    public Klass(int input) {
        // Second `@Inject` constructor.
    }
}
```

## Recommended
Make sure there is only one `@Inject` constructor in every class.


```java
import javax.inject.Inject;

public class Klass {
    @Inject
    public Klass() {
        // Firs `@Inject` constructor.
    }

    public Klass(int input) {
        // Second `@Inject` constructor.
    }
}
```
