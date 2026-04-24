# Boxed primitives should not be synchronized on
**ID:** `JAVA-E0150` | **Link:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-E0150)

![Major](https://img.shields.io/badge/severity-major-orange)![Bug Risk](https://img.shields.io/badge/type-bug_risk-green)

The code synchronizes on a boxed primitive constant, such as an `Integer`.


## Bad Practice

```java
private static Integer count = 0;

// ...

synchronized(count) {
    count++;
}
...
```
Since `Integer` objects constructed in this way can be cached and shared, this code could be synchronizing on the same object as other unrelated code, leading to unresponsiveness and possible deadlocks.


## Recommended

```java
private static int count = 0;
private final Object lock = new Object();

// ...

synchronized(lock) {
    count++;
}
```
