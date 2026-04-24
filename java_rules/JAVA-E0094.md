# Finalizers must not be explicitly invoked
**ID:** `JAVA-E0094` | **Link:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-E0094)

![Critical](https://img.shields.io/badge/severity-critical-red)![Anti-pattern](https://img.shields.io/badge/type-anti_pattern-purple)

This method explicitly invokes an object's `finalize` method. Because finalizer methods are supposed to be executed once, and only by the JVM's internal code, this is a bad idea.


## Bad Practice

```java
this.finalize();
```
If a connected set of objects is currently being finalized, there is a chance that the `finalize` method of all such objects could be called at the same time by the JVM over multiple threads. If the `finalize` method were to be invoked on such a connected object at the same time as its `finalize` method was called by the JVM, it could cause a virtually impossible to diagnose race condition.


## Recommended
Remove the call to `finalize`.

**Note:** Finalizers are deprecated since Java 9.

