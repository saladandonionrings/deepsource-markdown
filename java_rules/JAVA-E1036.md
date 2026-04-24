# Wrong argument type for Collection remove method
**ID:** `JAVA-E1036` | **Lien:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-E1036)

![Major](https://img.shields.io/badge/severity-major-orange) ![Bug Risk](https://img.shields.io/badge/type-bug_risk-green)

Arguments to collection `remove*` methods must be of the same type as the collection itself.

Though `Collection` is parameterised on the type of the contained values, [ `Collection.remove()` ](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/util/Collection.html#remove(java.lang.Object)) is not; it accepts a parameter of type `Object` instead. This means any value can be passed to a collection's `remove()` method, regardless of whether the value's type matches the collection's type.

This is also exacerbated for lists that store integers; `List` has both an `Object` and an `int` overload for `remove()` that are easy to confuse.


## Bad Practice

```java
List<Integer> ints = Arrays.asList(3);

ints.remove("3"); // this will fail silently!
```

## Recommended
Ensure that the type of the value passed to `remove()` is the same as the collection's type.


```java
ints.remove((Object)3);
```
