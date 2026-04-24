# Static fields of the parent class should not be accessed through child class instances
**ID:** `JAVA-W1030` | **Link:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-W1030)

![Major](https://img.shields.io/badge/severity-major-orange)![Anti-pattern](https://img.shields.io/badge/type-anti_pattern-purple)

Non-private static members of the parent class are accessible by child classes. However, it is a bad practice to do so, because it obscures where a value was actually declared. Always use only the declaring class to access static members.


## Bad Practice

```java
class SomeClass {
    static Object staticData = null;
}

class SomeChildClass {
    public void method() {
        // Accessing the static value declared within the parent class through the child class.
        SomeChildClass.staticData = this;
    }
}
```

## Recommended

```java
class SomeChildClass {
    public void method() {
        // We are now accessing the static value through the parent class.
        SomeClass.staticData = this;
    }
}
```
