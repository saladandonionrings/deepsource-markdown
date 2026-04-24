# Value assigned is overwritten due to switch fall through
**ID:** `JAVA-S0197` | **Link:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-S0197)

![Major](https://img.shields.io/badge/severity-major-orange)![Bug Risk](https://img.shields.io/badge/type-bug_risk-green)

A value stored in the previous switch case is overwritten here due to a switch fall through. It is likely that you forgot to put a break or return statement at the end of the previous case.


## Example

```java
// BAD

switch (val1) {
    case 1: 
        val2 = 3;
    case 2: 
        val2 = 2;
    default:
        val2 = 5;   // final value of val2 is 5 even if val1 == 1.
}

// GOOD

switch (val1) {
    case 1: 
        val2 = 3;
        break;
    case 2: 
        val2 = 2; 
        break;
    default:
        val2 = 5;
}
```
