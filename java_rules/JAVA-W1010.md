# The default case should be last within a switch block
**ID:** `JAVA-W1010` | **Link:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-W1010)

![Minor](https://img.shields.io/badge/severity-minor-yellow)![Anti-pattern](https://img.shields.io/badge/type-anti_pattern-purple)

`switch` blocks in Java do not impose a specific order for their clauses. This means that even if, say, the `default` clause were to appear as the first case in a switch block, Java will first ensure that no other switch case matches the input before executing it.

This does not mean it is all right to put `default` clauses just anywhere; as a convention, the `default` clause should only appear after any other clauses.


## Bad Practice
Placing the `default` clause anywhere else but at the end will not cause any bugs, but may confuse any person who reads such code. The usual expectation is that the `default` clause will be present at the end of the `switch` block.


```java
switch (someVar) {
    case VAL_A: ...
    default: ... // This may confuse readers of your code.
    case VAL_B: ...
}
```

## Recommended
Putting the `default` clause of a switch block at the end has been a long-standing tradition in `C`-like languages, and it is a tradition worth respecting for the clarity it provides.


```java
switch (someVar) {
    case VAL_A: ...
    case VAL_B: ...
    default: ... // Very visible.
}
```
