# removeAll should not be used to clear a collection
**ID:** `JAVA-P1005` | **Lien:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-P1005)

![Critical](https://img.shields.io/badge/severity-critical-red) ![Performance](https://img.shields.io/badge/type-performance-white)

This code appears to clear a collection by passing a reference of the collection into its own `removeAll()` method.

This is very inefficient, as it is an operation with complexity `O(n^2)` (quadratic time) as opposed to a regular `clear()` call which is `O(n)` (linear time) complex.

When one calls `a.removeAll(b)` where `a` and `b` are `Collection` s, we iterate over `a` , and check if `b` contains any element from `a` . If it does, we remove those elements. However, if we were to call `removeAll()` with `a` itself as its argument (like, `a.removeAll(a)` ), we would iterate once over `a` for each element within `a` . This is a very inefficient operation.

Additionally, calling `removeAll()` in this way on thread-safe collections may throw a `ConcurrentModificationException` in some cases.


## Bad Practice

```java
someCollection.removeAll(someCollection);
```

## Recommended
Just use `clear()` instead.


```java
someCollection.clear();
```
