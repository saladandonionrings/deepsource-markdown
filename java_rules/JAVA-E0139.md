# Waiting with two locks held is likely to cause a deadlock
**ID:** `JAVA-E0139` | **Link:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-E0139)

![Critical](https://img.shields.io/badge/severity-critical-red)![Bug Risk](https://img.shields.io/badge/type-bug_risk-green)

Waiting on a monitor while two locks are held may cause deadlock. This can also happen with `Lock` and `Condition` primitives from the `java.util.concurrent` package.


## Bad Practice
Nesting synchronized blocks and waiting on one value may cause deadlocks:


```java
synchronized(obj1) {
    
    // ...
    
    synchronized(obj2) {
        obj2.wait();

        // ...
    
    }

    // ...
}
```
Waiting on `obj2` does not release the lock on `obj1`. If any other code locks `obj2` before locking `obj1`, a deadlock will occur.

Similarly, it is not a good idea to hold multiple locks and call `await` on a `Condition` variable:


```java
Lock l = new ReentrantLock();
Lock l2 = new ReentrantLock();
Condition c = l.newCondition();
Condition c2 = l2.newCondition();

l.lock();
l2.lock();

// ...

c.await();

// ...

l2.unlock();
l.unlock();
```
Calling `c.await()` will only release `l`, not `l2`. Performing a wait only releases the lock associated with the wait operation, not any other locks.


## Recommended
If you must nest locks, be very careful about usage, and test your code rigorously. Deadlocks are dead serious business. It is almost always a bad idea to perform a wait operation when multiple locks are already held.

