# Synchronization performed on a util.concurrent `Lock` object
**ID:** `JAVA-S0321` | **Lien:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-S0321)

![Critical](https://img.shields.io/badge/severity-critical-red) ![Anti-pattern](https://img.shields.io/badge/type-anti_pattern-purple)

This method performs synchronization on an object that implements `java.util.concurrent.locks.Lock` .

Such an object is locked/unlocked using `acquire()` / `release()` rather than using the `synchronized (...)` construct.

Refactor the code to use the correct methods and constructs to achieve synchronization.

