# Consumed streams must not be reused
**ID:** `JAVA-E1019` | **Lien:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-E1019)

![Major](https://img.shields.io/badge/severity-major-orange) ![Bug Risk](https://img.shields.io/badge/type-bug_risk-green)

The usage of a stream object which has already been exhausted was detected. Reusing an exhausted stream object will lead to an `IllegalStateException` being thrown.


## Bad Practice
If any of the terminal stream operations, such as `forEach` , `min` , `max` or `allMatch` are used, the `Stream` object referenced by the target variable will be terminated. This means that no further operations would be possible, and further invocations of any operation will result in an exception:


```java
Stream<Integer> s = Stream.of(1, 2, 3);

Stream<Integer> b = s.filter(it -> it % 2 == 1);

b.forEach(System.out::println); // b is now exhausted!

b = b.map(it -> it * 2); // This fails with an IllegalStateException!
```

## Recommended
Structure your code carefully to avoid exhausting any stream object you use. `peek` can be useful when you need to iterate over elements of a stream while still leaving it usable for other operations.


```java
Stream<Integer> s = Stream.of(1, 2, 3);

Stream<Integer> b = s.filter(it -> it % 2 == 1);

// Now, we print each element before multiplying it, but we avoid terminating the stream as well!
b = b.peek(System.out::println).map(it -> it * 2);
```
