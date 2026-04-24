# Synchronization performed on a concurrency primitive object
**ID:** `JAVA-E0321` | **Lien:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-E0321)

![Major](https://img.shields.io/badge/severity-major-orange) ![Anti-pattern](https://img.shields.io/badge/type-anti_pattern-purple)

This method performs synchronization on an object that implements `java.util.concurrent.locks.Lock` .

Such an object is locked/unlocked using `acquire()` / `release()` rather than using the `synchronized (...)` construct.

Refactor the code to use the correct methods and constructs to achieve synchronization.


## Bad Practice
Consider a reentrant lock created somewhere:


```java
Lock someLock = new ReentrantLock();

// ...
```
Synchronizing on this lock is a wasteful operation, since the lock needn't have ever been created for this purpose.


```java
synchronized (someLock) {
    // ...
}
```

## Recommended
Use the lock's methods to synchronize your code instead:


```java
someLock.lock();

// ...

someLock.unlock();
```
If you'd like to preserve `synchronized` -style scoping, as well as automatic locking/unlocking of the lock, you could use one of the solutions provided [here](https://stackoverflow.com/a/46248923) .

