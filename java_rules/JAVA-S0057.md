# Maps and Sets of URLs can be performance hogs
**ID:** `JAVA-S0057` | **Link:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-S0057)

![Critical](https://img.shields.io/badge/severity-critical-red)![Performance](https://img.shields.io/badge/type-performance-white)

This method or field is or uses a `Map` or `Set` of `URL`s. Since both the `equals` and `hashCode` method of `URL` perform domain name resolution, this can result in a big performance hit.


## Examples

## Problematic Code

```java
HashMap<URL, Integer> hits = new HashMap<>();

// ...

for (HashMap.Entry<URL, Integer> e : hits) {
    // ... This can become very slow for larger hashmaps of URLS.
}
```

## Recommended
Consider using the `java.net.URI` class to represent URLs. This class does not have the same `hashCode` behavior, so it is safe to use as a key for map data structures.


```java
HashMap<URI, Integer> hits = new HashMap<>();

// ...

for (HashMap.Entry<URI, Integer> e : hits) {
    // ...
}
```
