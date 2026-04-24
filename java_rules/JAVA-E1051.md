# Synchronizing on a mutable reference may lead to unexpected behavior
**ID:** `JAVA-E1051` | **Lien:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-E1051)

![Major](https://img.shields.io/badge/severity-major-orange) ![Bug Risk](https://img.shields.io/badge/type-bug_risk-green)

This method is attempting to synchronize on a field whose value may change. This is very dangerous and may easily lead to difficult to diagnose bugs.


## Bad Practice
Consider the case of a mutable private field intended to be synchronized on:


```java
// The `Integer` constructor is deprecated, by the way.
private Integer monotonicCounter = new Integer(0);
```
And here is a method which attempts to acquire a lock on this field to provide mutual exclusion:


```java
private Integer countUp() {
     Integer result = null;
     synchronized(monotonicCounter) {
         result = new Integer(monotonicCounter + 1);
         // `monotonicCounter` now refers to a new `Integer` object on the heap.
         monotonicCounter = new Integer((int)result);
     }
     return result;
}
```
In the above code, `monotonicCounter` points to a new object each time `countUp()` is called. This means that any threads that called this function in the past may not have been synchronizing on the same object. The likelihood of 2 or more threads simultaneously changing the value of `monotonicCounter` is very high.


## Recommended
Use a dedicated object for synchronization purposes, and declare that object as `private final` :


```java
private final Object LOCK = new Object();
private int counter = 0;

// ...

private int countUp() {
     synchronized(LOCK) {
        counter += 1;
        return counter;
     }
}
```
Here, the lock object cannot be modified, so any two calls to the same method will always synchronize on the same object instance.

