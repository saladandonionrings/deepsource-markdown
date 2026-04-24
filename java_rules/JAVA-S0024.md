# Loops must terminate by some means
**ID:** `JAVA-S0024` | **Link:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-S0024)

![Critical](https://img.shields.io/badge/severity-critical-red)![Bug Risk](https://img.shields.io/badge/type-bug_risk-green)

This loop doesn't seem to have a way to terminate (other than by perhaps throwing an exception).

It is better to explicitly break out of the loop instead of relying on a possibly unclear exit condition.


## Examples

## Bad Practice

```java
while(true) {

    doSomething(...);

    // ...
}
```
It is inadvisable to break out of an infinite loop using an exception if that is what is intended; control flow expressions such as `break` and `return` exist for this purpose.


## Recommended

```java
while(true) {

    doSomething(...);

    if (somethingElse) break;
}
```
If this loop is not intentional, it may cause the application to hang unexpectedly.

