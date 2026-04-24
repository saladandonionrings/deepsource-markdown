# Finalizers are deprecated since Java 9
**ID:** `JAVA-W0088` | **Link:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-W0088)

![Major](https://img.shields.io/badge/severity-major-orange)![Anti-pattern](https://img.shields.io/badge/type-anti_pattern-purple)

Finalizers have been deprecated since Java 9. They are unreliable at best and can cause performance problems and even race conditions in certain cases. Remove the finalizer from your code if possible

Finalizers are often more trouble than they are worth. The JVM is not guaranteed to call any object's finalizer due to the way java performs garbage collections. Additionally, objects with finalizers and their descendents are treated differently when garbage collection is performed, leading to reduced GC performance. The degradation can manifest as increased latency, or long pauses for GC.

Because of the way Java's garbage collection works, objects without finalizers are eligible for GC sooner than objects with them. Also, the fields of objects with no finalizer may be garbage collected along with the object that contains them. Using a finalizer prevents such code from benefiting from such optimizations.


## Recommended
It is recommended to remove the finalizer, as they are seldom useful in general.

The more explicit `Cleaner` API could be used if functionality similar to a finalizer is required.

