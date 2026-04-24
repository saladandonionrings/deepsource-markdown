# `Hashtable/ConcurrentHashMap.contains()` checks for whether a value exists, not keys
**ID:** `JAVA-E1098` | **Link:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-E1098)

![Critical](https://img.shields.io/badge/severity-critical-red)![Bug Risk](https://img.shields.io/badge/type-bug_risk-green)

A call to [`Hashtable.contains()`](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/util/Hashtable.html#contains(java.lang.Object)) or to [`ConcurrentHashMap.contains()`](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/util/concurrent/ConcurrentHashMap.html#contains(java.lang.Object)) was detected where the object passed to the method was of the same type as the key of the concerned map. It is likely this should be replaced with a `containsKey` call instead.

[`Hashtable.contains()`](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/util/Hashtable.html#contains(java.lang.Object)) and [`ConcurrentHashMap.contains()`](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/util/concurrent/ConcurrentHashMap.html#contains(java.lang.Object)) are both legacy methods that check for whether the argument is present in the value set of the map structure, and not the key set.


## Bad Practice
Avoid using `contains()` to check for the presence of a key:


```java
ConcurrentHashMap<Integer, UUID> chm = ...;

// This would never be true, because a UUID is not an Integer!
if (chm.contains(32)) {
    // ...
}
```

## Recommended
To fix this problem, use the `containsKey()` method instead.


```java
if (chm.containsKey(32)) {
    // ...
}
```
