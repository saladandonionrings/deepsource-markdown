# `BigDecimal` constructed from `double` may be imprecise
**ID:** `JAVA-W0008` | **Lien:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-W0008)

![Major](https://img.shields.io/badge/severity-major-orange) ![Bug Risk](https://img.shields.io/badge/type-bug_risk-green)

`BigDecimal` s constructed from a `double` may not be represented correctly.

This code creates a `BigDecimal` from a `double` value that may not translate well to a decimal number. This happens due to the way real numbers are represented in binary. Only rational numbers that are powers of 2 can be represented with perfect accuracy in types such as `float` and `double` . For example, numbers such as `1/16` or `1/1024` are precisely representable whereas the binary representation of a number such as `1/10` would expand infinitely (similarly to how `1/3` 's decimal form expands infinitely when you try writing it down).

From `BigDecimal` 's [JavaDocs](https://docs.oracle.com/javase/7/docs/api/java/math/BigDecimal.html#BigDecimal(double)) :

One might assume that writing `new BigDecimal(0.1)` in Java creates a `BigDecimal` which is exactly equal to `0.1` (an unscaled value of 1, with a scale of 1), but it is actually equal to `0.1000000000000000055511151231257827021181583404541015625` .

For more information on why this occurs, see this [wikipedia article](https://en.wikipedia.org/wiki/Floating-point_arithmetic#Representable_numbers,_conversion_and_rounding)

You probably want to use the `BigDecimal.valueOf(double d)` method, which uses the `String` representation of the `double` to create the `BigDecimal` (e.g., `BigDecimal.valueOf(0.1)` gives `0.1` ).


```java
BigDecimal bad = new BigDecimal(0.1);

BigDecimal good = BigDecimal.valueOf(0.1);
```
