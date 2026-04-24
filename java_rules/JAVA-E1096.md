# `Iterable<Path>` is errorprone and should be replaced with `Collection<Path>`
**ID:** `JAVA-E1096` | **Lien:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-E1096)

![Major](https://img.shields.io/badge/severity-major-orange) ![Bug Risk](https://img.shields.io/badge/type-bug_risk-green)

Avoid using `Iterable<Path>` to represent collections of `Path` s.

[ `java.nio.file.Path` ](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/nio/file/Path.html) implements `Iterable` in and of itself, which allows paths to be treated as lists of path components. To avoid confusing a single `Path` object for a `List` (or whatever other data structure) of `Path` s, use `Collection` or a similar generic type instead.


## Bad Practice
In the example below, the for loop may iterate over `paths` as if it were a list of paths, but it is actually a single path split into its components.


```java
Iterable<Path> paths = Path.of("some/path");

// !!! this works!
for (Path toFile : paths) {
    // ...
}
```

## Recommended
To avoid accidentally allowing the usage of a single `Path` value where many are expected, you can use `Collection` classes, such as `List` , `Set` , or even `Collection` itself.


```java
Collection<Path> = List.of(Path.of("path/a"), Path.of("path/b"));

for (Path path : paths) {
    // ...
}
```
