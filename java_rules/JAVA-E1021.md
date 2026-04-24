# Do not synchronize on the result of `getClass()`
**ID:** `JAVA-E1021` | **Lien:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-E1021)

![Major](https://img.shields.io/badge/severity-major-orange) ![Bug Risk](https://img.shields.io/badge/type-bug_risk-green)

`Object.getClass()` returns the runtime type of a variable.

Because of this, it is not guaranteed that the same object will be returned for all values contained in a variable of a certain type, unless that type is declared as `final` .

Attempting to synchronize on the result of `getClass()` called on a variable with a non-final type could lead to concurrency bugs such as race conditions.


## Bad Practice

```java
class MyClass {
    // ...
}

class MySubClass extends MyClass {
    // ...
}

// ...

// Both of these values are declared as being of type MyClass...
MyClass firstExample = new MyClass();
MyClass secondExample = new MySubClass();

// firstExample.getClass() returns MyClass.class...
synchronized (firstExample.getClass()) {
    // ...
}

// secondExample.getClass() returns MySubClass.class !!!
synchronized (secondExample.getClass()) {
    // ...
}
```
While the declared type of `secondExample` is `MyClass` , its actual type at runtime is `MySubClass` . This means that we will not be synchronizing on the same object in the first and second `synchronized` blocks.


## Recommended
Use the static class instance of the specific type you wish to use instead:


```java
synchronized (MyClass.class) {
    // ...
}
```

## Exceptions
This issue will not be raised when the type of the value `getClass()` is used on is declared as `final` since a `final` type cannot be inherited from.

