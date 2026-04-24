# Unsynchronized lazy initialization of static value detected
**ID:** `JAVA-E1053` | **Lien:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-E1053)

![Critical](https://img.shields.io/badge/severity-critical-red) ![Bug Risk](https://img.shields.io/badge/type-bug_risk-green)

A static field has been lazy initialized without any synchronization used. This will allow race conditions to occur if the field's getter is called on multiple threads at once.


## Bad Practice

```java
public class UnsynchronizedLazyInit {
    private static SomeResource u = null;

    static SomeResource getResource() {
        // There is no synchronization used here...
        if (u == null) {
            u = new SomeResource();
        }

        return u;
    }
}
```
Here, if two threads were to call `getResource()` while `u` were `null` , both threads would attempt to assign a new `SomeResource` instance to `u` . In this scenario, one of the threads is likely to overwrite the value of `u` set in the other thread.


## Recommended
There are a number of ways to solve this issue.

**Use a synchronized method**

This solution may be problematic if some other code also synchronizes on `this` when a synchronized method is called.


```java
static synchronized SomeResource getResource() {
    if (u == null) {
        u = new SomeResource();
    }

    return u;
}
```
**Synchronize on a private lock variable**

This method is safer, since we are now using a private and final value which cannot be locked on directly by external code.


```java
private final Object LOCK = new Object();

static SomeResource getResource() {
    synchronized(LOCK) {
        if (u == null) {
            u = new SomeResource();
        }

        return u;
    }
}
```
