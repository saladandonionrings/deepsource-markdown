# Inefficient use of keySet iterator instead of entrySet iterator
**ID:** `JAVA-S0361` | **Link:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-S0361)

![Major](https://img.shields.io/badge/severity-major-orange)![Performance](https://img.shields.io/badge/type-performance-white)

This method accesses the value of a Map entry, using a key that was retrieved from a `keySet` iterator. It is more efficient to use an iterator on the `entrySet` of the map, to avoid the `Map.get(key)` lookup.


## Example

```java
// BAD
for (String key: map.keySet()) {
    ...
    if (satisfiesCriteria(key))
        value = map.get(key); // Inefficient
    ...
}

// GOOD
for (Map.Entry<String, Integer> entry : map.entrySet()) {
    ...
    if (satisfiesCriteria(entry.getKey())
        value = entry.getValue();
    ...
}
```
While the performance benefits of this change may not be very high for smaller maps, it is worth making this change if you will be handling maps with very large capacities (entry count in the millions for example), and/or slower or bad hashing implementations.

