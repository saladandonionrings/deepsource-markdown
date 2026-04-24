# `Thread` passed where `Runnable` expected
**ID:** `JAVA-E0056` | **Link:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-E0056)

![Major](https://img.shields.io/badge/severity-major-orange)![Anti-pattern](https://img.shields.io/badge/type-anti_pattern-purple)

A `Thread` object is passed as a parameter to a method where a `Runnable` is expected. This is rather unusual, and may indicate a logic error or cause unexpected behavior.

Because `Thread` inherits from `Runnable`, it has a public `run` method which any other code can freely call. In general, `Thread` wraps a `Runnable` instance, though it could be extended with a custom `run` method as well.

Calling `Thread.run` will not spawn a new thread; that is `Thread.start`'s responsibility. Such usage is not harmful in and of itself, but it is likely to raise eyebrows in code review. It may be that `Thread.run` was called in place of `Thread.start` by accident.


## Bad Practice

```java
Thread a = new Thread(new Runnable() {
    @Override
    public void run() {
        // ...
    }
});

a.run();
```

## Recommended

```java
a.start();
```
Or, if you intended to use `Runnable`,


```java
Runnable a = new Runnable() {
    @Override
    public void run() {
        // ...
    }
}

// ...

a.run(); // This is the same as Thread.run without the extra work.
```
Directly calling `Thread.run` will not spawn a new thread. If calling `run` is intentional, consider replacing the usage of `Thread` directly with `Runnable` instead, since that will allow for the same usage with less margin for error in future usage.

