# Synchronization on a nonfinal field is dangerous and error-prone
**ID:** `JAVA-S0154` | **Lien:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-S0154)

![Major](https://img.shields.io/badge/severity-major-orange) ![Bug Risk](https://img.shields.io/badge/type-bug_risk-green)

This method synchronizes on an object referenced from a mutable field. This can lead to concurrency bugs because different threads may end up synchronizing on different objects.


```java
// Lock field definition.
Object lock = new Object();

// ...

synchronized(lock) {
    // ...
}

// ...

// Any synchronization done on this lock object reference prior to this assignment will be independent of any future synchronization.
lock = new Object();
```
If at some point, the field is assigned a new value, any later attempts to synchronize on it will do so on a different monitor object. This is an easy recipe to introduce race conditions and should be avoided with extreme prejudice.

Ensure that the lock field is declared as `final` to make sure it cannot be modified.

