# Loops must terminate by some means
**ID:** `JAVA-E1046` | **Lien:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-E1046)

![Critical](https://img.shields.io/badge/severity-critical-red) ![Bug Risk](https://img.shields.io/badge/type-bug_risk-green)

This loop doesn't seem to have a way to terminate (other than by throwing an exception).

It is better to explicitly break out of the loop instead of relying on a possibly unclear exit condition.

This issue is triggered when the Java analyzer detects that the loop condition is always true, but does not contain any explicit flow control (such as a `break` or a `return` statement). This means that other than an exception thrown (possibly accidentally) by some call or operation somewhere within this loop, there is no other way to end it.


## Bad Practice

```java
while(true) {

    // maybe this throws an exception in some cases?
    doSomething(...);

    // ...
}
```
Even if you as the author of this code know that something will throw an exception and break out of the loop, others will not necessarily understand that (even with explanatory comments!). Control flow keywords such as `break` and `return` exist to state where a loop will exit.

If this is not intentional, it may cause the application to hang unexpectedly.


## Recommended

```java
while(true) {
    boolean breakout = doSomething(...);

    if (breakout) break;
}
```

## Exceptions
If this code is intentional (an event loop in embedded code for example), you may safely ignore this issue.

Make sure to explicitly record the reason for such a loop if the intent is not immediately clear.

