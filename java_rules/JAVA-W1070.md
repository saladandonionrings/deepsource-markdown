# Constructor must not be marked with nullable annotations
**ID:** `JAVA-W1070` | **Link:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-W1070)

![Major](https://img.shields.io/badge/severity-major-orange)![Anti-pattern](https://img.shields.io/badge/type-anti_pattern-purple)

Nullable annotations such as `@Nullable` and `@CheckForNull` on constructors is an antipattern.

In Java, constructors always return an instance of the class in which they are defined. Explicitly returning `null` from a constructor raises a compile-time error. In Java, it is impossible for a constructor to return `null`. Therefore, annotations such as `@Nullable` and `@CheckForNull` which are typically used to reflect that a method may return `null`, should be removed from constructors.


## Bad Practice

```java
public class Klass {
    @Nullable
    public Klass() {
        // ...
    }
}
```

## Recommended
Consider removing `@Nullable` and `@CheckForNull` from constructors.


```java
public class Klass {
    public Klass() {
        // ...
    }
}

## References
- StackOverflow - [Can constructors return a null object?](https://stackoverflow.com/questions/11103444/can-constructor-return-a-null-object)
```
