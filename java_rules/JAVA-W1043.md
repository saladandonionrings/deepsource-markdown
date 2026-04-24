# Bitwise operations should not be checked for sign
**ID:** `JAVA-W1043` | **Lien:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-W1043)

![Major](https://img.shields.io/badge/severity-major-orange) ![Bug Risk](https://img.shields.io/badge/type-bug_risk-green)

Avoid checking the sign of bitwise operations. If you need to compare an expression to `0` , just do an equality check with `0` . Otherwise, consider being more explicit by performing a bitwise test of the most significant bit instead.

Bitwise operations do not work well with comparison operations. This is because bitwise operations completely ignore the sign of any values involved, and do not produce results that agree with analogous arithmetic operations.


## Bad Practice
This code checks if the result of a bitwise `OR` is positive.


```java
if ((data & (1 << i)) > 0) {
    list.add("ERROR");
}
```
Using bitwise arithmetic and then comparing with the greater than operator can lead to unexpected results (of course depending on the value of `ExitCodes.ERROR_FLAG` as well as `exitCode` itself).


## Recommended
If you want to compare the value to 0, just use `!= 0` instead.


```java
if ((data & (1 << i)) == 0) {
    list.add("ERROR");
}
```
If this is intentional and should happen, consider being more explicit by testing the most significant bit instead. This works because Java's integers store negatives in 2's complement, which means positive numbers always have a `0` as the most significant bit.


```java
// 0x80000000 is a value where only bit 31 is 1 and all others are 0. It is equivalent to Integer.MIN_VALUE
if (((exitCode | ExitCodes.ERROR_FLAG) & 0x80000000) == 0) {
    // ...
}
```
