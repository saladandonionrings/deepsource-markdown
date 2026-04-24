# Serializable class with non-serializable superclass and no default constructor detected
**ID:** `JAVA-E1034` | **Link:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-E1034)

![Major](https://img.shields.io/badge/severity-major-orange)![Bug Risk](https://img.shields.io/badge/type-bug_risk-green)

This serializable class has a non-serializable superclass that does not declare a default constructor. Deserializing such a class will fail with an [`InvalidClassException`](https://docs.oracle.com/javase/8/docs/api/java/io/InvalidClassException.html) because Java will not be able to instantiate it.

Java's `Serializable` interface enforces specific requirements on serializable classes that extend a non-serializable class:

To allow subtypes of non-serializable classes to be serialized, the subtype may assume responsibility for saving and restoring the state of the supertype's public, protected, and (if accessible) package fields. The subtype may assume this responsibility only if the class it extends has an accessible no-arg constructor to initialize the class's state. It is an error to declare a class `Serializable` if this is not the case. The error will be detected at runtime.

Put simply, given the following conditions:

Java will throw an `InvalidClassException` when attempting to deserialize an instance of the class.


## Bad Practice

```java
class SuperClass {
    int x;
    public SuperClass(int a) {
        x = a;
    }
}

// Java will fail to deserialize this class.
class SubClass extends SuperClass implements Serializable {
    // ...
}
```

## Recommended

```java
class SuperClass {
    int x;
    public SuperClass(int a) {
        x = a;
    }

    public SuperClass() {
        x = 0;
    }
}

class SubClass extends SuperClass implements Serializable {
    // ...
}
```
