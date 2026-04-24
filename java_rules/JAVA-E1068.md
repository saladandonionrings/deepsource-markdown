# Random value in range 0 to 1 is coerced to integer 0
**ID:** `JAVA-E1068` | **Lien:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-E1068)

![Major](https://img.shields.io/badge/severity-major-orange) ![Bug Risk](https://img.shields.io/badge/type-bug_risk-green)

Casting the return value of methods such as `Random.nextFloat()` or `Math.random()` to an integral type such as `int` or `long` will always force the random value to be `0` .

[ `Random.nextFloat()` ](https://docs.oracle.com/javase/7/docs/api/java/util/Random.html#nextFloat()) , [ `Random.nextDouble()` ](https://docs.oracle.com/javase/7/docs/api/java/util/Random.html#nextDouble()) and (by way of using `Random.nextDouble()` internally) [ `Math.random()` ](https://docs.oracle.com/javase/7/docs/api/java/lang/Math.html#random()) all return random values within the range `\[0,1)` . When a floating point value is casted to an integer type, that value is rounded down to the next smallest integer value.

For any value between `0` and `1` , the next smallest integer value is `0` .

You probably want to multiply the random value by something else before coercing it to an integer, or use the [ `Random.nextInt(int)` ](https://docs.oracle.com/javase/7/docs/api/java/util/Random.html#nextInt(int)) method instead.


## Bad Practice

```java
long randVal = (long)someRandom.nextDouble();
```

## Recommended

```java
long randVal = someRandom.nextLong();
```
