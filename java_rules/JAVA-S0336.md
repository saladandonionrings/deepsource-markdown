# Assertion possibly occurs in a non-test thread
**ID:** `JAVA-S0336` | **Link:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-S0336)

![Major](https://img.shields.io/badge/severity-major-orange)![Bug Risk](https://img.shields.io/badge/type-bug_risk-green)

A JUnit assertion is performed in a `Runnable`'s run method, which may execute in a different thread from the one the test is running on.

Failed JUnit assertions just result in exceptions being thrown. Thus, if this exception occurs in a thread other than the thread that invokes the test method, the exception will terminate the thread but not result in the test failing.

Avoid assertions in non test threads. Consider refactoring your code to pass result values from other threads into the test thread and asserting only in the test thread.

