# Public fields should not be synchronized on
**ID:** `JAVA-E1061` | **Link:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-E1061)

![Major](https://img.shields.io/badge/severity-major-orange)![Bug Risk](https://img.shields.io/badge/type-bug_risk-green)

This code uses a public value as the monitor in a `synchronized` block.

Public values can be accessed from anywhere, and if some other code synchronizes on, or changes the value of such a public field, the chance of multithreading errors such as deadlocks and race conditions occurring is high.


## Bad Practice

```java
public Object somePublicObject;

// ... Elsewhere

synchronized (somePublicObject) {
    // ...
}
```

## Recommended
Use a private value which cannot easily be accessed externally instead.


```java
private Object somePrivateObject;


synchronized(somePrivateObject) {
    // ...
}
```
