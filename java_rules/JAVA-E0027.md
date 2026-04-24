# Elements accessed from volatile reference to an array are not volatile
**ID:** `JAVA-E0027` | **Link:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-E0027)

![Major](https://img.shields.io/badge/severity-major-orange)![Bug Risk](https://img.shields.io/badge/type-bug_risk-green)

A volatile reference to an array doesn't treat the array elements as volatile.

This code declares a volatile reference to an array, which might not be what you want.

With a volatile reference to an array, reads and writes of the reference to the array are treated as volatile, but the array elements are non-volatile. To get volatile array elements, you will need to use one of the atomic array classes in java.util.concurrent (available since Java 5.0).


## Bad Practice

```java
volatile Int[] bad = new Int[10](); // Does not guarantee coherence of reads/writes to its elements.
```

## Recommended
Using atomic classes is a good way to guarantee that all memory accesses across threads are coherent. However, because of the performance overhead, ensure that you absolutely need the concurrency guarantees before you use them.

It must be noted that an array of `AtomicInteger`s (`AtomicInteger[]`) is not the same as a single `AtomicIntegerArray`; the former is more prone to breakage and is a bad idea in general. `AtomicIntegerArray` stores ordinary integers and allows thread-safe access to them. An array of `AtomicInteger`s is a non-thread-safe data structure holding references to thread-safe integers.


```java
import java.util.concurrent.atomic.AtomicIntegerArray;

AtomicInteger[] arr = new AtomicInteger[10]();

arr[2] = new AtomicInteger(); // This is not visible across threads!

volatile AtomicInteger[] volArr = new AtomicInteger[10]();

volArr[2] = new AtomicInteger(); // This is also not visible across threads!

// This would be visible across all threads, 
// but it only applies when arr[2] points to an AtomicInteger and not null.
arr[2].set(3);
```
Changes to such an array will not be atomic or even coherent across threads unless all access to it is through a `synchronized` block or method.

