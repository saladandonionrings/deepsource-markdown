# Non-static nested class found
**ID:** `JAVA-W1019` | **Link:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-W1019)

![Minor](https://img.shields.io/badge/severity-minor-yellow)![Bug Risk](https://img.shields.io/badge/type-bug_risk-green)

This nested class is declared without a `static` modifier, meaning all instances of the class will hold a reference to an instance of the enclosing class.

The Java analyzer has detected that there are no explicit references to the enclosing class here, meaning this nested class can be safely treated as static.

Consider adding a `static` modifier to the nested class.

Non-static nested classes are generally known as "inner" classes.


## Bad Practice

```java
class Outer {

    class Inner {
        // ...
    }

    // ...
}
```
There are a number of things one should be aware of when using inner classes:

* An instance of`Inner`will contain a reference to an instance of`Outer`.If the`Inner`instance continues to exist after all other references to the`Outer`instance are deleted, the`Outer`instance will still exist because of the reference held by the still-alive`Inner`class.* If the`Inner`instance continues to exist after all other references to the`Outer`instance are deleted, the`Outer`instance will still exist because of the reference held by the still-alive`Inner`class.* The syntax for instantiating a nested class is relatively obscure, and may confuse future code maintainers.* Referring to private fields of the enclosing class from the nested class requires Java to generate synthetic accessor methods for that sole purpose, bloating the class's bytecode.
* If the`Inner`instance continues to exist after all other references to the`Outer`instance are deleted, the`Outer`instance will still exist because of the reference held by the still-alive`Inner`class.

## Recommended
Declare the nested class as static if possible.


```java
class Outer {
    static class Inner {
        // ...
    }

    // ...
}
```

## Exceptions
If the inner class refers to the outer class's instance fields, this issue will not be reported.

