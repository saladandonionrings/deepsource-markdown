# Expensive methods should not be invoked from performance critical threads
**ID:** `JAVA-P1007` | **Link:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-P1007)

![Major](https://img.shields.io/badge/severity-major-orange)![Performance](https://img.shields.io/badge/type-performance-white)

Performance critical threads shouldn't be blocked by expensive jobs.

Methods annotated with `@MainThread`, `@UIThread`, or `@PerformanceCritical` generally execute on threads that shouldn't be blocked by expensive jobs. Doing so might affect the overall responsiveness of the application.

`@Expensive` and `@WorkerThread` are used to annotate methods that are expensive to execute. Hence such methods must not be invoked from methods marked as `@MainThread`, `@UIThread`, or `@PerformanceCritical`.


## Bad Practice

```java
@WorkerThread // or @Expensive
void doExpensiveJob() {
 // ..expensive task goes here
}

@MainThread // or @UIThread, or @PerformanceCritical
public void someMethod() {
    doExpensiveJob();
}
```

## Recommended
Consider executing the task in a worker thread.


```java
@MainThread // or @UIThread, or @PerformanceCritical
public void someMethod() {
    workerThreadPool.submitTask(new Runnable() {
        public void run() {
            doExpensiveJob();
        }
    });
}
```
