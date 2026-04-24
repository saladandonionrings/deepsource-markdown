# Thread instances should not be used to call static methods of Thread
**ID:** `JAVA-E1062` | **Lien:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-E1062)

![Critical](https://img.shields.io/badge/severity-critical-red) ![Bug Risk](https://img.shields.io/badge/type-bug_risk-green)

A call to a static method of `Thread` through a single instance of the class has been detected. This may not work as expected and could cause unintended side effects.

Most of `Thread` 's static methods operate on the current thread. Thus, even if such a method is called from a `Thread` instance, only the currently active thread (the thread that is running the code you are looking at) will actually be affected by the method call.


## Bad Practice
Consider the example of checking if a thread is interrupted, using [ `Thread.interrupted()` ](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/lang/Thread.html#interrupted()) :


```java
boolean isSomeThreadInterrupted = someThread.interrupted();
```
Though it seems like this call is checking if `someThread` is in an interrupted state, it is in fact checking if the current active thread (which is executing the code you see above) is interrupted. Also note that `Thread.interrupted()` is not idempotent; it will check, and reset the `interrupted` flag of the current thread. Thus, two consecutive calls to `interrupted()` may not always return the same value.


## Recommended
Only call static methods of `Thread` through the class instance of `Thread` to avoid misunderstandings. `Thread` also has instance methods which may serve to achieve the same goal without accidentally changing the state of the thread:


```java
isSomeThreadInterrupted = someThread.isInterrupted();
```
Here, `isInterrrupted` is an instance method that will not affect thread state when called, meaning it is idempotent.

