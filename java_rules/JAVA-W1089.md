# For loop can be converted into a foreach loop
**ID:** `JAVA-W1089` | **Link:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-W1089)

![Minor](https://img.shields.io/badge/severity-minor-yellow)![Anti-pattern](https://img.shields.io/badge/type-anti_pattern-purple)

If a for loop can be converted to a foreach loop, consider doing so, as it is a more concise and readable syntax.

This issue is raised when the Java analyzer detects that all elements of a list/array are being iterated over, in sequence, and only one element of the iterable is accessed in one loop iteration.


## Bad Practice

```java
for (int i = 0; i < list.size(); i++) {
    SomeType value = list.get(i);

    // do whatever is required with value.
}
```

## Recommended
Use the `foreach` syntax to iterate over the iterable instead.


```java
for (SomeType value : list) {
    // Do the required operation.
}
```
