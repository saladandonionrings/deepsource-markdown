# Maximum pool size of `ScheduledThreadPoolExecutor` cannot be changed
**ID:** `JAVA-W0012` | **Link:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-W0012)

![Major](https://img.shields.io/badge/severity-major-orange)![Anti-pattern](https://img.shields.io/badge/type-anti_pattern-purple)

It is not possible to change the max pool size of a `ScheduledThreadPoolExecutor` using the setter functions inherited from `ThreadPoolExecutor`.

From `ScheduledThreadPoolExecutor`'s [JavaDocs](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/util/concurrent/ScheduledThreadPoolExecutor.html):

While `ScheduledThreadPoolExecutor` inherits from `ThreadPoolExecutor`, a few of the inherited tuning methods are not useful for it. In particular, because it acts as a fixed-sized pool using `corePoolSize` threads and an unbounded queue, adjustments to `maximumPoolSize` have no useful effect.

This may be contrary to assumptions the programmer may make, leading to subtle logic errors.


## Bad Practice

```java
ScheduledThreadPoolExecutor ste = new ScheduledThreadPoolExecutor(2);
ste.setMaximumPoolSize(4); // Doesn't work
```

## Recommended
Check how `ScheduledThreadPoolExecutor` is used and rewrite the code to obviate the need to change the max pool size.

If it is possible to avoid the need to resize the thread pool, try that approach.

