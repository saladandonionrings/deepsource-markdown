# `Thread.sleep()` should not be called while a lock is held
**ID:** `JAVA-E0410` | **Link:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-E0410)

![Major](https://img.shields.io/badge/severity-major-orange)![Bug Risk](https://img.shields.io/badge/type-bug_risk-green)

This method calls `Thread.sleep()` with a lock held. This may result in very poor performance and scalability, or a deadlock, since other threads may be waiting to acquire the lock.


## Bad Practice

```java
synchronized(something) {
    // ...


    sleep(20); // May cause a deadlock!!!

    // ...
}
```
Here is an example of this issue with a concurrency API (from the `java.util.concurrent` package) abstraction instead:


```java
lock.lock();

// ...

Thread.sleep(...);

// ...

lock.unlock();
```

## Recommended
When using monitor style synchronization, it is a better idea to call `wait()` on the lock, which releases the lock and allows other threads to run.


```java
synchronized(something) {
    // ...
    
    something.wait();
    
    // ...
}
```
If you are using locks or semaphores provided by the `java.util.concurrent` package, use the `await()` (for `Condition`s) or `acquire()` (for `Semaphore`s) methods instead.


```java
lock.lock();

// ...

cond.await();

// ...

lock.unlock();
```
