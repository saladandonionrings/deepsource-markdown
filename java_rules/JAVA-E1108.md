# Avoid using `ThreadGroup` methods
**ID:** `JAVA-E1108` | **Lien:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-E1108)

![Major](https://img.shields.io/badge/severity-major-orange) ![Bug Risk](https://img.shields.io/badge/type-bug_risk-green)

Most methods from the `ThreadGroup` class are unsafe and are marked as deprecated due to the way they affect global JVM state.

Avoid using them, and switch to safer alternatives from the `java.util.concurrent` package.

In Effective Java, Joshua Bloch states:

Thread groups are best viewed as an unsuccessful experiment, and you should simply ignore their existence.

The possible problems caused by the usage of this class include:

* Deadlocks If a thread group is suspended while one of its threads holds a lock, it could cause a deadlock by preventing that lock's release.
* If a thread group is suspended while one of its threads holds a lock, it could cause a deadlock by preventing that lock's release.
* Resource leakage If a thread group is destroyed, any resources that were used in its threads may not have been released, leading to resource exhaustion.
* If a thread group is destroyed, any resources that were used in its threads may not have been released, leading to resource exhaustion.
* Data races If a suspended thread is resumed at the wrong time, it is possible to allow it to access data it was not supposed to access.
* If a suspended thread is resumed at the wrong time, it is possible to allow it to access data it was not supposed to access.

* If a thread group is suspended while one of its threads holds a lock, it could cause a deadlock by preventing that lock's release.

* If a thread group is destroyed, any resources that were used in its threads may not have been released, leading to resource exhaustion.

* If a suspended thread is resumed at the wrong time, it is possible to allow it to access data it was not supposed to access.

This issue will be raised upon usage of the following methods:

* [ `ThreadGroup.stop()` ](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/lang/ThreadGroup.html#stop())
* [ `ThreadGroup.suspend()` ](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/lang/ThreadGroup.html#suspend())
* [ `ThreadGroup.resume()` ](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/lang/ThreadGroup.html#resume())
* [ `ThreadGroup.destroy()` ](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/lang/ThreadGroup.html#destroy())
* [ `ThreadGroup.isDestroyed()` ](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/lang/ThreadGroup.html#isDestroyed())
* [ `ThreadGroup.setDaemon()` ](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/lang/ThreadGroup.html#setDaemon(boolean))
* [ `ThreadGroup.isDaemon()` ](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/lang/ThreadGroup.html#isDaemon())
* [ `ThreadGroup.checkAccess()` ](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/lang/ThreadGroup.html#checkAccess())
* [ `ThreadGroup.allowThreadSuspension()` ](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/lang/ThreadGroup.html#allowThreadSuspension(boolean))


## Bad Practice

```java
// in main thread
try {
    someThreadGroup.stop();
} catch (...) {
    ...
}
```

## Recommended
Use alternatives from the `java.util.concurrent` package, or similar.

* `ThreadGroup.setDaemon()/isDaemon()` You could use a [ `ThreadFactory` ](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/util/concurrent/ThreadFactory.html) that is configured to create daemon threads instead.
* `ThreadGroup.suspend()/resume()` Rewrite your code to use locks (either from `java.util.concurrent` or with `synchronized` ) instead of using these methods.
* `ThreadGroup.destroy()/isDestroyed()` Avoid using these methods; instead, structure your code to allow for graceful shutdown by notifications.

