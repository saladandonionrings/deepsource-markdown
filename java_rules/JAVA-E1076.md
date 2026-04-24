# Collections should not contain themselves
**ID:** `JAVA-E1076` | **Lien:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-E1076)

![Critical](https://img.shields.io/badge/severity-critical-red) ![Bug Risk](https://img.shields.io/badge/type-bug_risk-green)

Avoid checking if a collection contains itself.

This may be a logical error caused by a typo. Check if you intended to do this.


## Bad Practice
In the example below, a membership check is done with the `strings` list, but it ends up checking if the list contains itself. Perhaps the `string` value a couple of lines above was intended to be used instead.


```java
List<String> strings = List.of("some", "strings");

String string = "...";

// Perhaps `string` was intended here...
if (strings.contains(strings)) {
    // ...
}
```

## Recommended
Verify if the `contains` call's argument is correct.

If the names of the variables in question are similar enough to be confusing, consider changing one of the variables to be more distinct.


```java
if (strings.contains(stringElement)) {
    // ...
}
```
