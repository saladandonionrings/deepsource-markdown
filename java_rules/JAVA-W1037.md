# Local variables should not be assigned in return statements
**ID:** `JAVA-W1037` | **Link:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-W1037)

![Major](https://img.shields.io/badge/severity-major-orange)![Anti-pattern](https://img.shields.io/badge/type-anti_pattern-purple)

A local variable is being assigned within a return statement. This may be either a typo in a comparison or an unnecessary operation.

Review this code and either directly return the local variable or convert the assignment into a comparison as intended.


## Bad Practice
Consider the following two (rather contrived) examples:

**Typos**


```java
Boolean checkForSomething(int s) {
    return s = 3;
}
```
Here, this method returns a `Boolean` value, but its return statement appears to return the result of assigning an `int` value. This code will fail to compile. It is likely that this was caused by the omission of an extra `=` character, turning the comparison into an assignment.

**Useless operations**


```java
Boolean return4() {
    int s;

    return s = 4;
}
```
Here, the method returns an integer value, but its return statement assigns to `s` within the return statement itself. This works because when treated as expressions, assignments evaluate to the assigned value.

This is also needless, since the same effect could be achieved by simply returning the assigned value directly.


## Recommended
If the assignment is a typo, just change the `=` into a more suitable operator.


```java
Boolean checkForSomething(int s) {
    return s != 3;
}
```
If it is just a superfluous operation, just return the assigned expression:


```java
Boolean return4() {
    return 4;
}
```
In this case, `s` can be removed entirely, since its only usage is within the erroneous assignment.

