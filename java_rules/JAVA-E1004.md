# Wait/notify must not be called on a Thread object
**ID:** `JAVA-E1004` | **Link:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-E1004)

![Major](https://img.shields.io/badge/severity-major-orange)![Bug Risk](https://img.shields.io/badge/type-bug_risk-green)

This code calls `wait` or `notify` on a `Thread`. If the main thread calls `join` on an instance of this thread, strange things may happen.

[`Thread.join`](https://docs.oracle.com/en/java/javase/16/docs/api/java.base/java/lang/Thread.html#join()) internally synchronizes on the thread object and calls `wait` in a loop until the thread has finished executing. If the thread is synchronized on elsewhere, the behavior of this multithreaded code may not be possible to determine.


## Bad Practice

```java
Thread t1 = new Thread(() -> {

    Thread t = Thread.currentThread();
    synchronized(t) {

    // ...

        t.wait(); // This could cause problems.

    // ...
    }
});

t1.start();

// elsewhere

t1.join();
```

## Recommended
Do not synchronize on thread objects. Use other objects (perhaps an `Object` instance specifically meant for synchronization) to do so instead.


```java
private final Object LOCK = new Object();

// ...

synchronized(LOCK) {

    // ...
    if (...) {
        LOCK.wait();
    }
}
```
