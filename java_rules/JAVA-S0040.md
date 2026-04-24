# `IllegalMonitorStateException`s should not be handled
**ID:** `JAVA-S0040` | **Link:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-S0040)

![Critical](https://img.shields.io/badge/severity-critical-red)![Anti-pattern](https://img.shields.io/badge/type-anti_pattern-purple)

`IllegalMonitorStateException` is generally only thrown in case of a design flaw in your code (calling `wait` or `notify` on an object you do not hold a lock on).

Handling such exceptions instead of diagnosing the underlying issue could lead to more bugs in the long run.

Do not attempt to catch and handle `IllegalMonitorStateException`. Instead, diagnose the reason for the exception's occurrence and fix the issue.

