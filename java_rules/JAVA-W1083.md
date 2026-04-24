# `BigDecimal.equals()` may produce unintended results
**ID:** `JAVA-W1083` | **Link:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-W1083)

![Major](https://img.shields.io/badge/severity-major-orange)![Bug Risk](https://img.shields.io/badge/type-bug_risk-green)

The Java analyzer has found a usage of `BigDecimal.equals()` in the code. This method may produce unintended results; it will also compare the scales of the two values, which can lead to it returning false even if the two values are equivalent.

The scale of a `BigDecimal` value can be thought of as the number of digits to the right of the decimal point in the value.


## Bad Practice
Consider two `BigDecimal` values:


```java
BigDecimal bd = BigDecimal.valueOf(3200);

// 32 * 10^-(-2) in terms of the input values.
BigDecimal bd2 = BigDecimal.valueOf(32, -2);
```
`bd` has a scale of `0`, while `bd2` has a scale of `-2`.

Now, if we compare the two values with the `equals()` method, we would find that it returns `false`, because the scales aren't the same.


```java
assertTrue(bd.equals(bd2)); // throws.
```

## Recommended
Use the `compareTo()` method instead. This method correctly compares the actual values of the two numbers, and will return `0` if the two numbers are numerically identical.


```java
assertEquals(0, bd.compareTo(bd2)); // Works.
```
