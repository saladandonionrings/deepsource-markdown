# `readResolve` should be protected for non-final classes
**ID:** `JAVA-W1097` | **Lien:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-W1097)

![Major](https://img.shields.io/badge/severity-major-orange) ![Anti-pattern](https://img.shields.io/badge/type-anti_pattern-purple)

The `readResolve` method provides additional control of how an object is deserialized.

If this method is made private for a non-final class, any child classes which are deserialized may end up missing deserialization logic that is implemented only in the parent class's private `readResolve` method.

The autofix for this issue will replace the private modifier with a protected modifier. If you instead wish to make the declaring class final, avoid applying the autofix.


## Bad Practice

```java
public class SomeClass implements Serializable {
    private Object readResolve() {
        // ...
    }
}
```

## Recommended
If the class should not have any child classes, make it final.


```java
public final class SomeClass implements Serializable {
    private Object readResolve() {
        // ...
    }
}
```
If the class is allowed to be inherited, make the `readResolve` method protected.


```java
public class SomeClass implements Serializable {
    protected Object readResolve() {
        // ...
    }
}
```
