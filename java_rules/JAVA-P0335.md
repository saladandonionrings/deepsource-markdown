# Inefficient use of `toArray` with non-zero sized array argument
**ID:** `JAVA-P0335` | **Lien:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-P0335)

![Major](https://img.shields.io/badge/severity-major-orange) ![Performance](https://img.shields.io/badge/type-performance-white)

This method uses `toArray` with a non-zero sized array argument. This is less efficient than passing a zero-sized array.

This method invokes `toArray` on a `Collection` object, and passes in an array with a size greater than zero as an argument. It used to be that this was faster than providing an array of size zero ( `new Type[0]` ) in older versions of Java prior to 6. This is because the cost of performing reflection operations was quite high in old Java versions.

This is no longer the case, and providing a zero sized array is now just as fast (or even faster) than providing an array of the same size as the original collection. It is now generally better to use a zero-length array as the argument to `toArray` .


## Bad Practice

```java
List<Integer> list = Arrays.asList(1, 2, 3, 4, 5, 6, 7, 8, 9, 10);

Integer[] arr1 = list.toArray(new Integer[list.size()]); // Inefficient
```

## Recommended

```java
Integer[] arr2 = list.toArray(new Integer[0]);           // Better
```
