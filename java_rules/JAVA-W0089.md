# Finalizer method should be protected, not public or private
**ID:** `JAVA-W0089` | **Lien:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-W0089)

![Critical](https://img.shields.io/badge/severity-critical-red) ![Anti-pattern](https://img.shields.io/badge/type-anti_pattern-purple)

A class's `finalize` method should not be publicly accessible, or completely private for that matter.

Change the access modifier to protected instead.


## Bad Practice
Finalizers are meant to be accessed only by the JVM, and are required to be protected by contract. This is because each class's `finalize` method is required to call its superclass's finalizer (and be called by any of its subclasses' finalizers) all the way to `Object.finalize` and so, needs to be overridable.


```java
@Override
public void finalize() {
    // ...
}

// OR

@Override
private void finalize() {
    // ...
}
```

## Recommended

```java
@Override
protected void finalize() {
    // ...
}
```
Always make the finalizer `protected` . It should be noted that finalizers are deprecated on Java versions above 9, and it is advised to move to the more predictable `Cleaner` API if functionality similar to a finalizer is required.

