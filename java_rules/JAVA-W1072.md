# Detected use of the `Date` API
**ID:** `JAVA-W1072` | **Lien:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-W1072)

![Major](https://img.shields.io/badge/severity-major-orange) ![Anti-pattern](https://img.shields.io/badge/type-anti_pattern-purple)

`java.util.Date` API should be avoided.

Use of the old `Date` API has been the source of many bugs in various Java programs. The design of this API is heavily criticised by the Java community. Some notable oddities include:

Therefore, it is highly recommended to use an alternative API to work with date and time.


## Recommended
Replace all usage of `Date` with [Clock](https://docs.oracle.com/javase/8/docs/api/java/time/Clock.html) if you are on Java version greater than 8. For earlier Java versions, consider using [Joda-Time](https://www.joda.org/joda-time/) .

