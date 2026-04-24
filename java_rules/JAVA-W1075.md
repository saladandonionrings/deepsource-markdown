# Abstract or default method annotated with `@Inject`
**ID:** `JAVA-W1075` | **Lien:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-W1075)

![Major](https://img.shields.io/badge/severity-major-orange) ![Anti-pattern](https://img.shields.io/badge/type-anti_pattern-purple)

Abstract or default methods should not be annotated with `@Inject` .

As per the JSR-330 specification, `@Inject` shouldn't be applied to abstract and default methods. When marking an abstract method as `@Inject` able, the programmer believes that all the methods that inherit from it will be injectable. This is not the case at all; a method that only inhertis from an `@Inject` method but isn't itself marked as `@Inject` will not be injected. So is the case with default methods defined in interfaces. For this reason, it is advised that you only use the `@Inject` annotation on concrete methods.


## Bad Practice

```java
import javax.inject.Inject;

abstract public class Klass {
    @Inject
    public abstract int doStuff();
}
```
or in `default` methods:


```java
import javax.inject.Inject;

public interface IFace {
    @Inject
    default public int doStuff() {
        //..default implementation here
    }
}
```

## Recommended
Remove `@Inject` from abstract and default methods. Instead, apply them only on concrete implementations.


```java
import javax.inject.Inject;

abstract public class Klass {
    public abstract int doStuff();
}

// Somewhere else in the codebase.
public class ConcreteKlass extends Klass {
    @Inject
    @Override
    public int doStuff() {
        // Implementation follows.
    }
}
```
