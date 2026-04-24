# Iterator `next` method should throw `NoSuchElementException`
**ID:** `JAVA-W0146` | **Lien:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-W0146)

![Major](https://img.shields.io/badge/severity-major-orange) ![Anti-pattern](https://img.shields.io/badge/type-anti_pattern-purple)

This class implements the `java.util.Iterator` interface. However, its `next()` method is not capable of throwing `java.util.NoSuchElementException` .

This is a violation of the `Iterator` interface's contract, and will not work with code that expects `next()` to throw when the iterator is exhausted.

The `next()` method should be changed so it throws `NoSuchElementException` if is called when there are no more elements to return.


## Bad Practice
This is a nonconforming implementation and may mislead API consumers.


```java
// Within iterator implementation
@Override
public T next() {
    if (hasNext()) { ... }
    else return null;
}
```

## Recommended
This implementation should be preferred:


```java
@Override
public T next() {
    if (hasNext()) { ... }
    else throw NoSuchElementException();
}
```
If the iterator will never throw, it may be preferable to write `hasNext()` to always return `true` , while throwing if `hasNext()` returns false. Obviously that would never occur, but it can serve to convey the intent. Always document such behavior for consumers of your API.


```java
@Override
public boolean hasNext() {
    return true;
}
```
Otherwise, a `NoSuchElementException` must be thrown to ensure conformance with the `Iterator` API.

