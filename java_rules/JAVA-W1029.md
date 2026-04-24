# Static methods should be accessed using the class instance
**ID:** `JAVA-W1029` | **Link:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-W1029)

![Major](https://img.shields.io/badge/severity-major-orange)![Anti-pattern](https://img.shields.io/badge/type-anti_pattern-purple)

While it is possible to access static members of a class through an instance of that class, it is a bad practice to do so.

Always access a static member through the declaring class itself, not an instance of the class.


## Bad Practice

```java
class SomeClass {
    static boolean somethingHappened = false;

    public void someMethod() {
        // Static value accessed through an object.
        this.somethingHappened = true;
    }
}
```

## Recommended
Access the value through the class itself.


```java
public void someMethod() {
        SomeClass.somethingHappened = true;
    }
```
