# `Thread` with empty `run` method is useless
**ID:** `JAVA-W0084` | **Lien:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-W0084)

![Major](https://img.shields.io/badge/severity-major-orange) ![Anti-pattern](https://img.shields.io/badge/type-anti_pattern-purple)

A thread was created using the default empty run method.

This method creates a thread with the default empty `run` method. When this thread is launched, it will exit as soon as it is scheduled since there is no code to run.


## Bad Practice

```java
Thread t = new Thread();

// The launched thread does nothing.
t.start();
```

## Recommended

```java
Thread t = new Thread(
    new Runnable {
        @Override
        void run() {
            // ...
        }
    }
);
```
Make sure to provide a valid `Runnable` instance to the `Thread` constructor while initializing it.

