# Optional values must be checked before being accessed
**ID:** `JAVA-E1013` | **Lien:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-E1013)

![Major](https://img.shields.io/badge/severity-major-orange) ![Bug Risk](https://img.shields.io/badge/type-bug_risk-green)

Do not call `Optional.get()` without first confirming whether there is a valid value present through `Optional.isPresent()` .

`java.util.Optional` is a very useful tool for avoiding the usage of `null` in a codebase. However, even an `Optional` can be unsafe if it does not contain a value when used.

If `get()` is called without calling `isPresent()` , an exception could be raised, which would mean the `Optional` is no better than a normal nullable value.


## Bad Practice

```java
Optional<Integer> result = someComputation();

// Will throw if result is empty.
int intVal = result.get();

result = Optional.empty();

result.get(); // Throws!
```

## Recommended
There are 2 ways to fix this:

Only use `get()` after a condition that checks if the `Optional` value is valid.


```java
if (!result.isPresent()) return;

int intVal = result.get();
```
Another useful feature of `Optional` is the `ifPresent()` method, which accepts a lambda taking the value of the `Optional` as an argument:


```java
result.ifPresent(value -> {
    // ...
});
```
