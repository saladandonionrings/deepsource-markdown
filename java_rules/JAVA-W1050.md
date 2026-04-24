# Primitives do not need to be boxed for comparison
**ID:** `JAVA-W1050` | **Link:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-W1050)

![Minor](https://img.shields.io/badge/severity-minor-yellow)![Performance](https://img.shields.io/badge/type-performance-white)

A boxed primitive is created just to call its `compareTo` method. It's more efficient to use the associated static compare method (for double and float since Java 1.4, for other primitive types since Java 7) which works on primitives directly.


## Bad Practice

```java
// This expression can be simplified to directly compare the primitive values instead.
Integer.valueOf(3).compareTo(2)

// or
new Integer(3).compareTo(2)
```

## Recommended
Use the static `compare()` method of the corresponding type instead.


```java
int compareResult = Integer.compare(3, 2);
```
