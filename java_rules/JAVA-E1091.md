# `Class.isInstance()` should not be called on a `Class` object
**ID:** `JAVA-E1091` | **Link:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-E1091)

![Major](https://img.shields.io/badge/severity-major-orange)![Bug Risk](https://img.shields.io/badge/type-bug_risk-green)

Passing [`Class.isInstance()`](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/lang/Class.html#isInstance(java.lang.Object)) an object of type `java.lang.Class` won't behave as expected.


## Bad Practice

```java
SomeObject so = new SomeObject();

if (so.getClass().isInstance(SomeObject.class)) { // Bad!
    // ...
}
```
The `isInstance()` method is designed to check if an object is an instance of the receiver type. However, when `isInstance()` is called on a `Class` object, it will always return `false`, as `java.lang.Class` (the type of the argument to `isInstance()`) is not a subclass of `SomeObject`.


## Recommended
Instead of passing the class of the object, pass the object itself:


```java
// Pass the object directly!
if (SomeObject.class.isInstance(so)) {
    // ...
}
```
