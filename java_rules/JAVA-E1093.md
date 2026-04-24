# `TimeZone.getTimeZone` should be passed correct timezone IDs
**ID:** `JAVA-E1093` | **Lien:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-E1093)

![Major](https://img.shields.io/badge/severity-major-orange) ![Bug Risk](https://img.shields.io/badge/type-bug_risk-green)

[ `java.util.TimeZone.getTimeZone()` ](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/util/TimeZone.html#getTimeZone(java.lang.String)) should not be passed invalid time zone identifier strings, as this will make it fail silently and return GMT instead.

Java cannot check for invalid timezones at compile time, so it is up to the developer to ensure that the string provided is actually valid. To avoid accidentally using an invalid time zone, it is recommended to select from a known list of timezone identifiers, such as those provided by the [ `ZoneId.getAvailableZoneIds()` ](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/time/ZoneId.html#getAvailableZoneIds()) method.


## Bad Practice

```java
TimeZone tz = TimeZone.getTimeZone("invalid/timezone"); // tz is assigned the GMT time zone.
```

## Recommended
Use a valid timezone string.


```java
TimeZone tz = TimeZone.getTimeZone("America/New_York");
```
