# Unsupported JDK-internal APIs should not be used
**ID:** `JAVA-E1030` | **Lien:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-E1030)

![Major](https://img.shields.io/badge/severity-major-orange) ![Bug Risk](https://img.shields.io/badge/type-bug_risk-green)

Java 9 [introduced a number of changes to the language](https://blogs.oracle.com/java/post/whats-cool-in-java-8-and-new-in-java-9) , and [deprecated or restricted access](https://stackoverflow.com/questions/47645924/jdk-9-unsafe-import-sun-misc-launcher) to a number of internal (and undocumented) Java and Sun APIs. Such APIs must not be used, and usages of classes from affected packages must be replaced by equivalent alternative APIs.

This issue will be raised if any restricted/private Java APIs, which were undocumented but still accessible in Java versions <= 8 are used in a project that uses a Java version >= 9.


## Bad Practice

```java
import jdk.internal.dynalink.DynamicLinker; // This API is undocumented and not meant to be used publicly.
```

## Recommended

```java
// Java provides the jdk.dynalink module as a more properly supported alternative.
import jdk.dynalink.DynamicLinker;
```
