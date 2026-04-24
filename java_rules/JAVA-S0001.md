# `@OverridingMethodsMustInvokeSuper` annotation in super method is ignored by overriding method
**ID:** `JAVA-S0001` | **Link:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-S0001)

![Major](https://img.shields.io/badge/severity-major-orange)![Bug Risk](https://img.shields.io/badge/type-bug_risk-green)

The super method is annotated with `@OverridingMethodsMustInvokeSuper`, but the overriding method isn't calling the super method.

This can cause bugs since there may be logic that depends on the super method being called, hence the super method being marked with the `@OverridingMethodsMustInvokeSuper` annotation.


## Examples

```java
class Super {

    @OverridingMethodsMustInvokeSuper
    void method() {
        // ...
    }
}
```

## Bad Practice

```java
class Bad extends Super {

    @Override
    void method() {
        // ...
    }
}
```

## Recommended

```java
class Good extends Super {

    @Override
    void method() {

        // This could be anywhere in the method as required, but it should exist.
        super.method();

        // ...
    }
}
```
Make sure to call the super method at some point in the overriding method.

