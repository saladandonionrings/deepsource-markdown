# Bad short-circuiting null check
**ID:** `JAVA-E1003` | **Link:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-E1003)

![Critical](https://img.shields.io/badge/severity-critical-red)![Bug Risk](https://img.shields.io/badge/type-bug_risk-green)

This short-circuiting boolean expression using either of the `&&` or `||` operators attempts to check if a variable is `null` in its left hand side, and then proceeds to call a method of that variable in its right hand side. However, because of the way it has been written, this expression will allow the right hand side to execute even if the variable is `null`. This will likely result in a `NullPointerException` at runtime.

This can occur when the programmer is confused about the behavior of short-circuiting operators such as `&&` and `||`. Here's a primer on how short-circuit operators interact with null-checks.

Consider this expression:


```java
a == null || a.someMethod()
```
The expression first checks if `a` is `null`. If it is, there is no need to execute the RHS, since a boolean `OR` operation only requires either of the conditions to be true. Since the LHS condition is proven to be true, there is no need to execute the RHS.

If `a` is not null, the RHS must also be checked, since we now care about the truth of the RHS.

Using the `&&` operator works similarly:


```java
a != null && a.someMethod()
```
We first check if `a` is not equal to `null`. If `a` is `null`, the LHS evaluates to `false`. This means the RHS will not need to be evaluated, and thus prevents an NPE. If `a` isn't `null` the RHS will also need to be evaluated. Why? Because `&&` requires both sides to be `true` to evaluate to `true`. But as long as even the LHS evaluates to `false`, `&&` will evaluate to `false`.


## Bad Practice

```java
String a = null;
// This code will pass over the LHS and directly execute the RHS, causing an NPE.
if (a != null || a.length() > 3) {
    // ...
}

// This code will fail similarly.
if (a == null && a.length() < 2) {
    // ...
}
```

## Recommended

```java
// Using the || operator
if (a == null || a.length() > 3) {
    // ...
}

// Using the && operator
if (a != null && a.length() < 2) {
    // ...
}
```
