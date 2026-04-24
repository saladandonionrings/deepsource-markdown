# Inject annotations on abstract class constructors have no effect
**ID:** `JAVA-W1084` | **Lien:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-W1084)

![Major](https://img.shields.io/badge/severity-major-orange) ![Anti-pattern](https://img.shields.io/badge/type-anti_pattern-purple)

The constructor of an abstract class can never be called directly by the dependency injection framework, meaning any injection annotations applied to it will not be considered. Remove the annotation.


## Bad Practice
Here, the `@Inject` annotation has no use, as the constructor will be ignored by DI.


```java
abstract class SomeAbstractClass {

    @Inject
    public SomeAbstractClass(SomeDependency val1) {
        // ...
    }
    
    // ...
}
```

## Recommended
Remove the annotation.

