# Sequence of operations on a concurrent abstraction may not be atomic
**ID:** `JAVA-S0447` | **Link:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-S0447)

![Critical](https://img.shields.io/badge/severity-critical-red)![Bug Risk](https://img.shields.io/badge/type-bug_risk-green)

This code contains a sequence of calls to a concurrent abstraction (such as a concurrent hash map). These calls will not be executed atomically.


## Bad Practice

```java
final ConcurrentMap<Integer, Integer> m = new ConcurrentSkipListMap<>();


// ...

// Some thread.
// The sequence of actions will not be executed as one atomic operation as it is.
m.put(3 ,4);
m.put(4, 2);
m.get(4);
```

## Recommended
It's better to use synchronization (not necessarily with `synchronized`) to achieve atomicity:


```java
synchronized (m) {
    m.put(3 ,4);
    m.put(4, 2);
    m.get(4);
}
```
