# Oddness check using `x % 2 == 1` will not work for negative numbers
**ID:** `JAVA-E0405` | **Lien:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-E0405)

![Major](https://img.shields.io/badge/severity-major-orange) ![Bug Risk](https://img.shields.io/badge/type-bug_risk-green)

The code uses an equivalent of `x % 2 == 1` to check to see if a value is odd, but this won't work for negative numbers. If this code is intending to check for oddness, consider using `x & 1 == 1` , or `x % 2 != 0` .

Using a check like `x % 2 == 1` will evaluate differently when `x` is negative than when `x` is positive:


```java
213 % 2  //  1

-213 % 2  // -1 !!!

 212 % 2  //  0

-212 % 2  //  0
```

## Bad Practice

```java
boolean badIsOdd(int x) {
    return x % 2 == 1;
}

assertTrue(BadIsOdd(-1)); // fails...
```

## Recommended
Replace the equality check using `1` with an inequality check using `0` instead:


```java
boolean goodIsOdd(int x) {
    return x % 2 != 0;
}
```
