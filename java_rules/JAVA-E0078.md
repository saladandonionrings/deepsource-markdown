# Monitor `wait` must not be used on a `Condition`
**ID:** `JAVA-E0078` | **Link:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-E0078)

![Major](https://img.shields.io/badge/severity-major-orange)![Bug Risk](https://img.shields.io/badge/type-bug_risk-green)

This method calls `wait` on a `java.util.concurrent.locks.Condition` object. Waiting for a `Condition` should be done using one of the `await` methods defined by the `Condition` interface. This may have occurred due to a typo omitting the "`a`" in `await`.


## Bad Practice

```java
Condition c = someLock.newCondition();

// This won't work.
c.wait();
```

## Recommended

```java
// This is the proper function to call.
c.await();
```
If this is intentional, consider refactoring your code to not use java monitor style methods such as `wait` or `notify`, especially when you are already using standard library concurrency classes as well.

