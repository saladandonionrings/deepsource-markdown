# `Iterable` objects must not return `this` in `iterator()` method
**ID:** `JAVA-E1015` | **Lien:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-E1015)

![Major](https://img.shields.io/badge/severity-major-orange) ![Bug Risk](https://img.shields.io/badge/type-bug_risk-green)

Do not return `this` in the `iterator()` method of a type that implements `java.lang.Iterable<T>` .

<!-more-->

`java.lang.Iterable<T>` represents a collection of values which can be iterated over. It can be iterated over any number of times.

`java.util.Iterator<T>` represents a stateful "cursor" over an iterable collection. It can only be iterated over once.

If you need to attach an [ `Iterator` ](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/util/Iterator.html) API to an `Iterable` structure, one way to do so would be to have the type implement both `Iterable` and `Iterator` , and return `this` in the [ `iterator()` ](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/lang/Iterable.html#iterator()) method:


```java
/// This type implements both Iterator and Iterable.
public class SomeCollection implements Iterator<T>, Iterable<T> {

    private T[] inner = ...;
    private int idx = 0;

    // Iterable.iterator() implementation
    @Override
    public Iterator<T> iterator() {
        return this;
    }

    @Override
    public boolean hasNext() {
        return idx < inner.length;
    }

    @Override
    public T next() {
        return inner[idx++];
    }

    // ...
}
```
It is a bad idea to do this however; getting an iterator from `SomeCollection.iterator()` will likely only work once. Consider what would happen when an instance of `SomeCollection` is iterated over more than once:


```java
SomeCollection si = new SomeCollection();

for (T i : si) {
    // ...
}

// This loop never executes!
for (T j : si) {
    // ...
}
```
The second loop will never execute! This is because once the first loop finishes, the internal index variable `idx` is equal to the length of the internal array `inner` . The `hasNext()` method will always return `false` and thus the second loop will never execute.

In certain cases such behavior is desirable: consider [ `java.nio.file.DirectoryStream` ](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/nio/file/DirectoryStream.html) . `DirectoryStream` is an `Iterable` over file paths that is intended to only be traversed once. It does so by ensuring that the `iterator()` method will always throw an exception after it is first called.


```java
DirectoryStream<Path> ds = Files.newDirectoryStream(somePath);

for (Path entry : ds) {
    // ...
}

// An IllegalStateException will be thrown here.
for (Path reEntry : ds) {
    // ...
}

ds.close();
```
Always return a fresh `Iterator` instance when implementing `Iterable<T>.iterator()` .


## Bad Practice

```java
class SomeIterable implements Iterable<SomeElement>, Iterator<SomeElement> {
    private SomeElement[] internalList = new SomeElement[10];
    private int idx = 0;
    private int capacity = 10;

    @Override
    public boolean hasNext() {
        return idx < capacity;
    }

    @Override
    public SomeElement next() {
        return internalList[idx++];
    }

    // Don't return this here!
    @Override
    public Iterator<SomeElement> iterator() {
        return this;
    }

    // ...
}
```

## Recommended
Create a new `Iterator` instance whenever the `iterator()` method is called. This will prevent state from persisting across invocations of the method.


```java
class SomeIterable implements Iterable<SomeElement> {
  private SomeElement[] internalList;

  public Iterator<SomeElement> iterator() {
    return new Iterator<SomeElement>() {
      private idx = 0;
      public boolean hasNext() {
        return idx < internalList.length;
      }
      public SomeElement next() {
        return internalList[idx++];
      }
    };
  }

  // ...
}
```
