# Iterator `hasNext` should not invoke `next`
**ID:** `JAVA-E0409` | **Link:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-E0409)

![Critical](https://img.shields.io/badge/severity-critical-red)![Bug Risk](https://img.shields.io/badge/type-bug_risk-green)

This implementation of `hasNext` calls the iterator's `next` method. This is wrong because `hasNext` must not cause the state of the iterator to be modified, while `next` usually does modify iterator state.

This will also result in subtle errors caused by missed items (`hasNext` calls will consume items of the iterator) and premature exhaustion of the iterator due to too many calls to `hasNext`.


## Bad Practice

```java
class MyIterator extends Iterator<Integer> {

    int i = 1;

    @Override
    boolean hasNext() {
        return next() < Integer.MAX_VALUE;
    }

    @Override
    Integer next() {
        return ++i;
    }
}
```

## Recommended
Change the implementation to perform the hasNext check in a manner that does not modify iterator state.


```java
class MyIterator extends Iterator<Integer> {

    int i = 1;

    @Override
    boolean hasNext() {
        // We now check the state of the iterator instead of the value returned by next.
        return i < Integer.MAX_VALUE;
    }

    @Override
    Integer next() {
        return ++i;
    }
}
```
