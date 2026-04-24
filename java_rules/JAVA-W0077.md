# `Object.getClass` does not need to be invoked on an instantiated object
**ID:** `JAVA-W0077` | **Link:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-W0077)

![Minor](https://img.shields.io/badge/severity-minor-yellow)![Anti-pattern](https://img.shields.io/badge/type-anti_pattern-purple)

This method allocates an object just to call `getClass()` on it, in order to retrieve the `Class` object for it. It is simpler to just access the static `.class` property of the class itself.


## Bad Practice

```java
Class<SomeClass> c = new SomeClass().getClass();
```

## Recommended

```java
Class<SomeClass> c = SomeClass.class;
```
Just use the static `.class` property when you can statically determine the class object you need.

