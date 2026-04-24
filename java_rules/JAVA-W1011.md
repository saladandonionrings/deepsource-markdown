# Map compute methods cannot be used to create null valued entries
**ID:** `JAVA-W1011` | **Lien:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-W1011)

![Major](https://img.shields.io/badge/severity-major-orange) ![Anti-pattern](https://img.shields.io/badge/type-anti_pattern-purple)

When `Map` 's `compute` , `computeIfAbsent` and `computeIfPresent` methods are provided with a lambda that always returns `null` , they will fail to create new entries for the provided key.

This may go against the expected behavior. Consider changing this code to directly insert a key with a null value instead.

The compute methods are a convenient and powerful way to perform certain actions that are predicated on whether an element exists in the map or not.

If a null value is returned from the lambda provided to these methods, no new element will be created for the corresponding key, which may not be the assumed behavior.


## Bad Practice

```java
Map<String, String> someMap = new HashMap<>();

someMap.computeIfAbsent("key", key -> null);
someMap.computeIfPresent("key", (key, value) -> { return null; });
someMap.compute("key", (key, oldValue) -> null);

assertEquals(0, someMap.size()); // Passes.
```
All three of the calls above make no changes to the map because no new entry is created by the methods.


## Recommended
If you intend to create a new null valued entry in the map, use `Map.put()` to insert the entry:


```java
if (!someMap.containsKey("key")) {
    someMap.put("key", null);
}

assertEquals(1, someMap.size());
```
