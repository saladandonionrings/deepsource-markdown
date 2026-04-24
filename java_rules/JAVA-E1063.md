# Double assignment of variable detected
**ID:** `JAVA-E1063` | **Lien:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-E1063)

![Major](https://img.shields.io/badge/severity-major-orange) ![Bug Risk](https://img.shields.io/badge/type-bug_risk-green)

A double assignment of a variable to itself has been detected. This may be a typo.

Check whether this is correct and edit it or remove the extra assignment.

When used as an expression, an assignment evaluates to the result of the RHS expression. For example, in the assignment `a = 3` , Java would evaluate the result of the assignment as `3` .


## Bad Practice
Assigning a value to itself is redundant and does not achieve any benefits over assigning just the value of the RHS expression.


```java
someVar = someVar = ...; // redundant!
```

## Recommended
It is likely that this was a typo. Perhaps the intention was to use a different variable in place of one of the repeated names.


```java
someVar = someOtherVar = ...;
```
**Alternatives**

Consider changing this double assignment into two single, separate assignments on different lines.


```java
someVar = ...;

someOtherVar = someVar;
```
This can have multiple benefits:

* It is clear where an assignment occurs.
* It is easier to understand what value is being assigned to what variable.
* It improves readability.

