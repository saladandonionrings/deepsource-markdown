# `switch` statements with only 2 branches should be `if` statements instead
**ID:** `JAVA-W1086` | **Lien:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-W1086)

![Major](https://img.shields.io/badge/severity-major-orange) ![Anti-pattern](https://img.shields.io/badge/type-anti_pattern-purple)

`switch` statements that have only two arms can be better represented as `if` statements.

If the intent is to add more cases later on, consider adding a `// skipcq: JAVA-W1086` to the top of the switch block to avoid reporting the issue.


## Bad Practice

```java
switch (someInt) {
    case 1 -> action1();
    default -> elseAction();
}
```

## Recommended
Just use an `if-else` block:


```java
if (someInt == 1) {
    action1();
} else {
    elseAction();
}
```

## Exceptions
This issue will not be reported for switch blocks that have multiple non-default arms.

