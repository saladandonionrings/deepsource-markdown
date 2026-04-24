# Iterators should not be invalidated while in scope
**ID:** `JAVA-E1085` | **Link:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-E1085)

![Major](https://img.shields.io/badge/severity-major-orange)![Bug Risk](https://img.shields.io/badge/type-bug_risk-green)

Collections should not be modified when an iterator is still in scope.

If a collection is modified while there is an iterator for that collection in scope, it is possible that inconsistencies could be introduced, leading to bugs. Depending on the context and the collection being used, such code can also result in a [`ConcurrentModificationException`](https://docs.oracle.com/en/java/javase/17/docs/api/java.base/java/util/ConcurrentModificationException.html) being thrown. It is thus highly recommended that you avoid any structural modification in a collection while any of its iterators are still in scope.


## Bad Practice

```java
Iterator<String> it = listOfStrings.iterator();
listOfStrings.clear();

String first = it.next(); // Will throw an ConcurrentModificationException!
```
Consider avoiding operations that add, remove, or replace elements in a collection until all its iterators go out of scope.


## Recommended

```java
Iterator<String> it = listOfStrings.iterator();
// use of the iterator follows
}
```
