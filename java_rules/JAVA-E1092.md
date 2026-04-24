# `ZoneId.of()` should be passed a valid timezone identifier
**ID:** `JAVA-E1092` | **Link:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-E1092)

![Major](https://img.shields.io/badge/severity-major-orange)![Bug Risk](https://img.shields.io/badge/type-bug_risk-green)

[`java.time.ZoneId.of()`](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/time/ZoneId.html#of(java.lang.String)) should not be passed invalid time zone identifier strings, as this will cause exceptions to be thrown at runtime.

`ZoneId` doesn't check the input string at compile-time, so it is up to the developer to ensure that the string provided is actually valid. To avoid errors, it is recommended to select from a known list of timezone identifiers, such as those provided by the [`ZoneId.getAvailableZoneIds()`](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/time/ZoneId.html#getAvailableZoneIds()) method.


## Bad Practice

```java
ZoneId zoneId = ZoneId.of("invalid/timezone");
```
Here, an invalid timezone string is passed to `ZoneId.of()`. This would cause a `ZoneRulesException` to be thrown.


## Recommended
Use a valid timezone string.


```java
ZoneId zoneId = ZoneId.of("America/New_York");
```
