# `wait`/`notify` called without synchronization on an object
**ID:** `JAVA-E0288` | **Lien:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-E0288)

![Critical](https://img.shields.io/badge/severity-critical-red) ![Bug Risk](https://img.shields.io/badge/type-bug_risk-green)

This method calls one of `Object.wait` , `notify` or `notifyAll` on a value that does not appear to be synchronized on in the current thread/method. Calling these methods on an object without a lock held on it will result in an `IllegalMonitorStateException` (or IMSE for short) being thrown.

This issue will be raised if a method contains a call to `Object.wait()` , `Object.notify()` or `Object.notifyAll()` without a corresponding `synchronized` block surrounding it. While it may be that the `synchronized` block is present somewhere higher in the call stack, the method implementation should never rely on the implied presence of a `synchronized` block in calling code, whether it is in private or (especially) in public API code.

Such design patterns can easily result in elusive bugs that occur in only very specific situations.


## Bad Practice
Here, there is no synchronization implemented on this method at all, and if it is called outside a synchronized block specifically synchronizing on `waitObj` , it will throw an IMSE.


```java
public void method(Object waitObj) {
    waitObj.wait(); // This will throw an IllegalMonitorStateException unless there is a synchronized block in calling code.
}
```
In this case, we are attempting to synchronize on `this` through a synchronized method. Within the method, we call `wait` on `waitObj` , which may point to `this` (that would make the call safe), but is not guaranteed to do so at all.


```java
public synchronized void method(Object waitObj) {
    waitObj.wait(); // This will throw an IMSE because we are synchronizing on this instead of on waitObj.
}
```

## Recommended

```java
public void method(Object waitObj) {
    synchronized(waitObj) {
        waitObj.wait(); // Since we have ensured that we have exclusive access to waitObj, an IMSE will not be thrown.
    }
}
```
