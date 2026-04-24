# Invalid values for `java.time` constants will always throw a `DateTimeException`
**ID:** `JAVA-E1099` | **Link:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-E1099)

![Critical](https://img.shields.io/badge/severity-critical-red)![Bug Risk](https://img.shields.io/badge/type-bug_risk-green)

Calling any method of the `java.time` package with invalid constant values will throw a [`DateTimeException`](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/time/DateTimeException.html).


## Bad Practice

```java
LocalDate date = LocalDate.of(1985, Month.MAY, 32);
```
In the code above, the `LocalDate` object is instantiated with a day-of-month value of `32`, which is a plainly invalid day of any month. This will result in a `DateTimeException` at runtime.


## Recommended
Use values within the valid range for the specific date/time field when calling such methods.


```java
LocalDate date = LocalDate.of(1985, Month.MAY, 31);
```
