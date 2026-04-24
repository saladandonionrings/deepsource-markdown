# Empty finalizer methods should be deleted
**ID:** `JAVA-W0090` | **Lien:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-W0090)

![Minor](https://img.shields.io/badge/severity-minor-yellow) ![Anti-pattern](https://img.shields.io/badge/type-anti_pattern-purple)

Empty `finalize` methods are useless, they should be deleted.


## Bad Practice

```java
@Override
protected void finalize() { }
```

## Recommended
Delete the method. It should be noted that finalizers are deprecated on Java versions above 9, and it is advised to move to the more predictable `Cleaner` API if functionality similar to a finalizer is required.

