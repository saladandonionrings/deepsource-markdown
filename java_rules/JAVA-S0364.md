# Float comparison with `NaN` will always fail
**ID:** `JAVA-S0364` | **Lien:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-S0364)

![Minor](https://img.shields.io/badge/severity-minor-yellow) ![Bug Risk](https://img.shields.io/badge/type-bug_risk-green)

This code checks to see if a floating point value is equal to the special. However, because of the special semantics of `NaN` , no value is equal to `Nan` , including `NaN` . Thus, `x == Double.NaN` always evaluates to false.


## Example

```java
// BAD
if (x == Double.NaN) { ... } // This condition will always fail.


assert(Double.NaN == Double.NaN); // Fails: NaN != NaN

// GOOD
if (Double.isNaN(x)) { ... }

// Alternatively for floats:
if (Float.isNaN(x)) { ... }
```
