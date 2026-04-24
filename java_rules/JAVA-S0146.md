# Iterator `next` method must throw `NoSuchElementException`
**ID:** `JAVA-S0146` | **Lien:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-S0146)

![Major](https://img.shields.io/badge/severity-major-orange) ![Anti-pattern](https://img.shields.io/badge/type-anti_pattern-purple)

This class implements the `java.util.Iterator` interface. However, its `next()` method is not capable of throwing `java.util.NoSuchElementException` .

This is a violation of the `Iterator` interface's contract, and will not work with code that expects `next()` to throw when the iterator is exhausted.

The `next()` method should be changed so it throws `NoSuchElementException` if is called when there are no more elements to return.


## Example
This is a bad implementation and may mislead API consumers.


```java
// Within iterator impl
@Override
public T next() {
    if (hasNext()) { ... } 
    else return null;
}
```
This implementation should be preferred:


```java
@Override
public T next() {
    if (hasNext()) { ... }
    else throw NoSuchElementException();
}
```
If such behavior is a requirement, a more preferable alternative is to extend `Iterator` and create a new interface whose contract allows this:


```java
public interface NonThrowingIterator extends Iterator { ... }
```
Otherwise, a `NoSuchElementException` must be thrown to ensure conformance with the `Iterator` API.

