# `@Inject` detected on a final field
**ID:** `JAVA-W1071` | **Lien:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-W1071)

![Major](https://img.shields.io/badge/severity-major-orange) ![Anti-pattern](https://img.shields.io/badge/type-anti_pattern-purple)

`@javax.inject.Inject` should not be used on final fields.

JSR-330 [forbids](https://docs.oracle.com/javaee/6/api/javax/inject/Inject.html) the use of `@Inject` on final fields. Many popular libraries such as Guice have adopted the convention and [disallowed @Inject on final fields](https://github.com/google/guice/wiki/Injections#field-injection) . It's highly encouraged to write code that follows this convention.


## Bad Practice

```java
import @javax.inject.Inject;

public class Klass {
    @Inject
    private final String name;
}
```

## Recommended
Just remove `@Inject` from `final` fields.


```java
public class Klass {
    private final String name;
}
```
