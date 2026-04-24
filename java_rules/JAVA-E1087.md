# `Duration.withNanos()` may not produce correct results
**ID:** `JAVA-E1087` | **Link:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-E1087)

![Major](https://img.shields.io/badge/severity-major-orange)![Bug Risk](https://img.shields.io/badge/type-bug_risk-green)

Using `Duration.withNanos()` may produce wrong results, because it will only set the value of the nanoseconds field of the duration, and will not correctly adjust for any overflow.


## Bad Practice

```java
Duration d = Duration.of(2, ChronoUnit.SECONDS);
// Any overflow from nanos to seconds will not be handled!
d = d.withNanos(extraNanoseconds);
```

## Recommended
Use the two-argument overload of [`Duration.ofSeconds()`](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/time/Duration.html#withNanos(int)) instead.


```java
Duration d = Duration.ofSeconds(2, extraNanoseconds);
```

## Exceptions
This rule will respect suppress annotations like `@SuppressWarnings("JavaDurationWithNanos")` applied to the enclosing block or declaration.

