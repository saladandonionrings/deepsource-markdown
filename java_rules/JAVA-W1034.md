# Conditions should not contain assignments
**ID:** `JAVA-W1034` | **Lien:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-W1034)

![Major](https://img.shields.io/badge/severity-major-orange) ![Bug Risk](https://img.shields.io/badge/type-bug_risk-green)

This condition seems to have an assignment (like `a = b` ) instead of a comparison (like `a == b` ).

Such code can be confusing and difficult to read and debug. Consider separating out the assignment and perform only a comparison within the expression.


## Bad Practice
While this code will compile, it will also likely perform the wrong operation, since `a` will always be treated as being `true` .


```java
if (a = true) {
    // ...
}
```
For other types, compilation will likely fail because in general, constructs such as `if` , `for` and `while` expect boolean expressions in their conditions, and will not accept other types.


## Recommended
Change the assignment to a comparison.


```java
if (a == true) {
    // ...
}
```
