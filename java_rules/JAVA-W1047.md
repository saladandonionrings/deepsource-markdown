# Useless control flow detected
**ID:** `JAVA-W1047` | **Link:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-W1047)

![Minor](https://img.shields.io/badge/severity-minor-yellow)![Anti-pattern](https://img.shields.io/badge/type-anti_pattern-purple)

This method contains a useless control flow statement, where control flow continues onto the same place regardless of whether the branch is taken or not. For example, this is caused by having an empty statement block for an `if` statement, or having a conditional that always fails or succeeds:


## Bad Practice

```java
// The condition may succeed but nothing happens.
if (argv.length == 0) {
    // TODO: handle this case
}

// The condition will always fail.
if (false) {

}

// The condition will always succeed.
if (true) {

}

// A loop with an empty body.
for (int i = 0; i < n; i++) {

}

// ...
```

## Recommended
Remove such if statements if they are not required.


## Exceptions
In some cases, loops are used as a way to exhaust or skip over elements in an iterator. This pattern is valid, and the Java analyzer will ignore empty loops where an iterator is updated in the condition.

