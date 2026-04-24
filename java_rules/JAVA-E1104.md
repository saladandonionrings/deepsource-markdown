# CacheLoader implementation `load` method should not return `null`
**ID:** `JAVA-E1104` | **Lien:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-E1104)

![Major](https://img.shields.io/badge/severity-major-orange) ![Bug Risk](https://img.shields.io/badge/type-bug_risk-green)

The `CacheLoader` interface's `load` method defines the action to perform when a value that is not present in a Guava `LoadingCache` is requested from the cache. `LoadingCache` requires the value returned by `CacheLoader.load()` to be a valid cache entry, and returning `null` will cause the cache to throw an `InvalidCacheLoadException` .

Ensure that an appropriate non-null value is returned instead.


## Bad Practice

```java
CacheLoader myLoader = new CacheLoader<SomeKey, SomeClass>() {
    @Override
    public SomeClass load(SomeKey key) throws Exception {
        // ...
        
        return null; // !!!
    }
}
```

## Recommended
Avoid returning `null` . Throw an appropriate exception instead.


```java
@Override
public SomeClass load(SomeKey key) throws Exception {
    // ...
    
    if (cantCreateValue) throw SomeException("cause");
    
    return validValue;
}
```
