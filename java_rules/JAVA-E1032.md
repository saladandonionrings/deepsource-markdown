# `readResolve` must return `Object`
**ID:** `JAVA-E1032` | **Lien:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-E1032)

![Major](https://img.shields.io/badge/severity-major-orange) ![Bug Risk](https://img.shields.io/badge/type-bug_risk-green)

`readResolve()` must return only `java.lang.Object` , not any other type.

`readResolve` is a useful optional component of Java's deserialization machinery, and can be used to customise the instance of a class created from deserialized data.


## Bad Practice
If you wish to use `readResolve` 's functionality, you must implement it with only the specific zero-argument, `Object` return type signature. This is because Java's serialization API looks specifically for a `readResolve` method that takes no arguments and returns `Object` . If such a method is not found, the JVM will default to creating a new instance with the default constructor.


```java
private MyClass readResolve() throws ObjectStreamException {
    return new MySubClass(); // return a specific type other than the default
}
```
This is specially relevant for deserialization of singleton objects, which rely on there being only one instance present throughout the lifetime of the application.

Consider this implementation of `readResolve` , which delegates to a singleton `getInstance` method when called:


```java
// Is ignored during deserialization!
public BadSingleton readResolve() throws ObjectStreamException {
    return BadSingleton.getInstance();
}
```
This method will be ignored when the singleton is deserialized, and so, the singleton will be recreated for every time deserialization occurs. Such requirements may come about when an application must save and restore its runtime state repeatedly (An Android application is a good example of such a scenario).


## Recommended
Use the correct signature when implementing `readResolve()` .


```java
private Object readResolve() throws ObjectStreamException {
    return new MySubClass(); // return a specific type other than the default
}
```
