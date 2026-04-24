# Avoid using deprecated `Thread` methods
**ID:** `JAVA-E1107` | **Lien:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-E1107)

![Major](https://img.shields.io/badge/severity-major-orange) ![Bug Risk](https://img.shields.io/badge/type-bug_risk-green)

Deprecated methods from `java.lang.Thread` such as `Thread.stop()` or `Thread.suspend()` should not be used as they can cause instability.

By using methods like `Thread.stop()` , any locks held within the affected thread will be released at once, possibly leading to inconsistencies and logical bugs.

This issue will be raised upon usage of the following methods:

* [ `Thread.stop()` ](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/lang/Thread.html#stop())
* [ `Thread.suspend()` ](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/lang/Thread.html#suspend())
* [ `Thread.resume()` ](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/lang/Thread.html#resume())


## Bad Practice

```java
// in main thread
try {
    someThread.stop();
} catch (...) {
    ...
}
```

## Recommended
The main thread can call a function on the worker side that determines if the worker should exit.


```java
class Main {
    // ...
    
    // in main thread:
    if (shouldQuitWorker) {
        worker.stop();
    }
    
    // ...
}
```
In the worker thread, you could check a flag to see if you should stop the thread.


```java
class Worker implements Runnable {
    // ...
    
    // Remember, we can't synchronize on a primitive, so we must box the value here.
    private Boolean stopFlag = false;
    
    @Overrride
    void run() {
        while (true) {
    
            // ...
    
            synchronized (stopFlag) {
                if (stopFlag) break;
            }

            // ...
        }
    }

    // ...

    public void stop() {
        synchronized(stopFlag) {
            stopFlag = true;
        }
    }
}
```
You could also use [ `Thread.interrupt()` ](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/lang/Thread.html#interrupt()) , which will set the interrupted state of the thread in question, to notify the target thread.

The main thread can call `Thread.interrupt()` to serve as a stop notification.


```java
class Main {
    // ...
    
    if (shouldQuitWorker) {
        workerThread.interrupt();
    }
    
    // ...
}
```
In the worker thread, you'd just need to check if the thread is interrupted.


```java
class Worker implements Runnable {
    // ...
    
    private Boolean stopFlag = false;
    
    @Overrride
    void run() {
        try {
            while (!Thread.currentThread().isInterrupted()) {
                // ...
            }
        } catch (InterruptedException e) {
            // ...
        } finally {
            // handle cleanup...
        }
    }
}
```
This will also cause side effects such as throwing an [ `InterruptedException` ](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/lang/InterruptedException.html) if the thread is currently blocked on a call to `wait()` , `join()` or `sleep()` , so be careful.

