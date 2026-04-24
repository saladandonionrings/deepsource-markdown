# `Instant` should not be passed unsupported temporal unit types
**ID:** `JAVA-E1094` | **Lien:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-E1094)

![Major](https://img.shields.io/badge/severity-major-orange) ![Bug Risk](https://img.shields.io/badge/type-bug_risk-green)

[ `java.time.Instant` ](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/time/Instant.html) methods like `plus()` , `minus()` and `until()` should not be passed invalid temporal units in their second argument, as this could cause a crash.

Only pass one of `NANOS` , `MICROS` , `MILLIS` , `SECONDS` , `MINUTES` , `HOURS` , `HALF_DAYS` and `DAYS` as the second argument to these methods.

These three methods of `Instant` only accept time units at a maximum scale of [ `ChronoUnit.DAYS` ](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/time/temporal/ChronoUnit.html#DAYS) , meaning that any units other than that will be rejected with an exception.


## Bad Practice

```java
Instant instant = Instant.now();
instant.plus(1, ChronoUnit.WEEKS); // You can't add weeks to an instant!
```
This code would trigger an `UnsupportedTemporalTypeException` at runtime.


## Recommended
Use only `ChronoUnit.DAYS` and below to perform operations on `Instant` objects.


```java
Instant instant = Instant.now();
instant.plus(1, ChronoUnit.HALF_DAYS);
```
