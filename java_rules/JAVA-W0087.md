# Finalizer nulls fields
**ID:** `JAVA-W0087` | **Lien:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-W0087)

![Minor](https://img.shields.io/badge/severity-minor-yellow) ![Anti-pattern](https://img.shields.io/badge/type-anti_pattern-purple)

This finalizer nulls out fields.


## Bad Practice

```java
public void finalize() {
    field1 = null;
    // ...
}
```
In older Java versions (way back in the days of 1.0 and 1.1!) nulling fields explicitly had its advantages, but this does not apply anymore.

Because of the way Java's garbage collection works, objects without finalizers are eligible for GC sooner than objects with them. Also, the fields of objects with no finalizer may be garbage collected along with the object that contains them. Nulling out fields means that the objects referenced by those fields will have to be finalized separately from the originally garbage collected object, since the garbage collector does not consider these field objects while GC-ing the object that contained them.


## Recommended
It is recommended to remove the finalizer, as they are seldom useful in general.

It should also be noted that finalizers are deprecated on Java versions above 9, and it is advised to move to the more predictable `Cleaner` API if functionality similar to a finalizer is required.

