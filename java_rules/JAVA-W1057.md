# Method can be declared static
**ID:** `JAVA-W1057` | **Link:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-W1057)

![Major](https://img.shields.io/badge/severity-major-orange)![Anti-pattern](https://img.shields.io/badge/type-anti_pattern-purple)

Private final methods that do not access instance fields should be declared static.

Since the method is private and final, it can't possibly be overridden. Therefore, the only possible definition of the method that exists doesn't access any instance fields. So we can safely declare it as static.


## Bad Practice

```java
public class Klass {
    private final int aField = 10;

    private final void method() {
        // ...statements that do not access `aField`
    }
}
```

## Recommended
Consider declaring the method static.


```java
public class Klass {
    private final int aField = 10;

    private static final void method() {
        // ...statements that do not access `aField`
    }
}
```
