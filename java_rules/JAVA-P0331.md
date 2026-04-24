# `Pattern.compile()` should not be called in a loop
**ID:** `JAVA-P0331` | **Link:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-P0331)

![Critical](https://img.shields.io/badge/severity-critical-red)![Performance](https://img.shields.io/badge/type-performance-white)

This method calls `Pattern.compile()` inside a loop with constant arguments. If this `Pattern` will be used several times, there's no reason to compile it on each loop iteration.

`Pattern.compile()` is an expensive method to run. This is because regexes are computationally intensive to generate. If regex compilation were to occur in a loop, the cumulative overhead from every iteration could cause an undesirable amount of lag.

Move this call outside of the loop or into a static final field.


## Bad Practice

```java
while (someCondition) {

    Pattern regex = Pattern.compile("(r|R)egex");

    // ...
}
```

## Recommended
If the regex will be used multiple times over the course of the program, it is better to declare it separately, perhaps as a static field.


```java
Pattern regex = Pattern.compile("(r|R)egex");

while (someCondition) {
    Matcher matches = regex.matcher(input);
    matches.find();
    // ...
}
```
If possible, turn the declaration into a static final field:


```java
static final Pattern regex = Pattern.compile("(r|R)egex");
```
