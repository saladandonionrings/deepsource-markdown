# Thread.currentThread() should not be used to call Thread's static methods
**ID:** `JAVA-W1038` | **Link:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-W1038)

![Major](https://img.shields.io/badge/severity-major-orange)![Anti-pattern](https://img.shields.io/badge/type-anti_pattern-purple)

This method invokes `Thread.currentThread()` just to call one of `Thread`'s static methods.

Most static methods of `Thread` operate on the current thread, so there is no need to explicitly get the current thread object to call them.


## Bad Practice

```java
boolean isInterrupted = Thread.currentThread().interrupted();
```

## Recommended
Just use the method with `Thread`'s class instance directly.


```java
isInterrupted = Thread.interrupted();
```
