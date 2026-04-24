# Boolean expression LHS/RHS with bitwise OR will never be equal to a constant RHS/LHS
**ID:** `JAVA-E1073` | **Link:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-E1073)

![Major](https://img.shields.io/badge/severity-major-orange)![Bug Risk](https://img.shields.io/badge/type-bug_risk-green)

This method compares an expression of the form `(e | C)` to `D`, where `e` is an expression and `C` & `D` are constant values. The comparison will always fail because of the specific values of `C` and `D`. This may indicate a logic error or typo.


## Bad Practice
Typically, this bug occurs because the code wants to perform a membership test in a bit set, but uses the bitwise OR operator (`|`) instead of bitwise AND (`&`).


```java
final int FLAG_1 = 0x00000020;

// This if statement will always fail.
if (value | FLAG_1 == 0) {
    // ...
}
```
Such bugs may also appear in expressions like `(e & A | B) == C`, which is parsed as `((e & A) | B) == C`; the actual intended expression may have been `(e & (A | B)) == C`.


## Recommended
Use the bitwise AND operator (`&`) to perform mask operations, not the bitwise OR operator (`|`).


```java
if (value & FLAG_1 == 0) {
    // if FLAG_1 is unset...
}
```
