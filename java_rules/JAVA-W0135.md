# Invoking a `Runnable` object's `run` method will perform the task in the current thread, not a new one
**ID:** `JAVA-W0135` | **Link:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-W0135)

![Major](https://img.shields.io/badge/severity-major-orange)![Bug Risk](https://img.shields.io/badge/type-bug_risk-green)

The `run` method of the referenced `Thread` was invoked. Did you mean to invoke `start` instead?

This code explicitly invokes `run` on an object. In general, classes implement the `Runnable` interface because they are going to have their `run` method invoked in a new thread, in which case `Thread.start` is the right method to call.

Calling `Runnable.run` directly will execute code meant to run on a separate thread on the same thread, blocking execution and breaking any code that expects the contents of the `run` method to be executed on a different thread.


```java
Thread a = new Thread(new Runnable() {

    @Override
    public void run() { ... }
  });
```

## Bad Practice

```java
a.run(); // Will not spawn a new thread!
```

## Recommended

```java
a.start(); // Will spawn a new thread.

  a.join(); // And this works as expected.
```
