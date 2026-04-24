# Impossible downcast of `toArray()` result detected
**ID:** `JAVA-S0386` | **Link:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-S0386)

![Critical](https://img.shields.io/badge/severity-critical-red)![Bug Risk](https://img.shields.io/badge/type-bug_risk-green)

This code is casting the result of calling `toArray()` on a collection to a subtype of `Object[]`, as in:


```java
String[] getAsArray(Collection<String> c) {
    return (String[]) c.toArray();
}
```
This will usually fail by throwing a `ClassCastException`. The `toArray()` method of almost all collections returns an `Object[]`. They can't really do anything else, since the `Collection` object does not have any way to determine its generic type.

The correct way to obtain an array of the desired type is by providing an empty array argument of the desired type:


```java
c.toArray(new String[]);
```
There is one common/known exception to this. The toArray() method of lists returned by `Arrays.asList(...)` will return a covariantly typed array. For example, `Arrays.asArray(new String[] { "a" }).toArray()` will return a `String []` instead of an `Object []`.

