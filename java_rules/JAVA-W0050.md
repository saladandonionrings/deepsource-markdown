# Use of identifier that is a keyword in later Java versions
**ID:** `JAVA-W0050` | **Link:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-W0050)

![Major](https://img.shields.io/badge/severity-major-orange)![Bug Risk](https://img.shields.io/badge/type-bug_risk-green)

This identifier is reserved as a keyword in later versions of Java. If/when this code is migrated to a newer Java version, it will not compile unless the identifier is renamed.

Keywords such as `enum`, `var` or `assert` were not always keywords. Older code that uses them as identifiers may break when ported to newer Java versions.

The following tokens used to be treated as identifiers but are now treated as keywords:

* `strictfp`- Since Java 1.2. Used to make floating point operations more portable across all Java platforms.* `assert`- Since Java 1.4* `enum`- Since Java 1.5
The tokens listed below on the other hand are "restricted", and only behave as keywords in certain contexts. Though it is still possible to use them as identifiers, the Java analyzer will raise warnings for such usages, as they could confuse readers of this code in the future.

* `yield`- Since Java 13. Allows the return value of`switch`blocks to be assigned to a variable.* `record`- Since Java 14. Used to declare record classes.* `var`- Since Java 10. Used to declare local variables. The type of the value is inferred.* `sealed`- Since Java 15. Marks an interface as being implemented by a restricted set of classes.* `permits`- Since Java 15. Used to specify the set of classes/interfaces which can directly implement/extend a particular sealed interface.
