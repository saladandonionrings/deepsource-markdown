# Variable is checked for `null` twice
**ID:** `JAVA-W1068` | **Link:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-W1068)

![Major](https://img.shields.io/badge/severity-major-orange)![Anti-pattern](https://img.shields.io/badge/type-anti_pattern-purple)

There is no point in repetitively checking if a variable is null if the variable hasn't been written between those checks.

Such checks are useless and can only lead to confusion. Therefore, consecutive null checks using methods such as `checkNotNull()`, `verifyNotNull()`, and `requireNonNull()` should be avoided.


## Bad Practice

```java
List<String> strings = getStrings();
Objects.requireNonNull(strings);
// ...a bunch of statements that don't modify the variable `strings`.
Objects.requireNonNull(strings); // Redundant check.
```

## Recommended
Consider removing redundant null checks.


```java
List<String> strings = getStrings();
Objects.requireNonNull(strings);
```
