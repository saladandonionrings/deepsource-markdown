# Nullable parameters should be checked for null before use
**ID:** `JAVA-E1067` | **Lien:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-E1067)

![Critical](https://img.shields.io/badge/severity-critical-red) ![Bug Risk](https://img.shields.io/badge/type-bug_risk-green)

This parameter is always used as if it is non-null, but the parameter may be null when the usage occurs.


## Bad Practice

```java
public void doWork(MyClass myClass) {
    // ...

    // Dereference without a null check.
    myClass.myField = "value";

    // ...
}

// ...

MyClass nullable;

// ...

if (condition) nullable = new MyClass();
else nullable = null;

// ...

doWork(nullable); // If nullable is null, we will get an NPE
```

## Recommended
If you require that the parameter should never be null, consider changing the annotation to `@NonNull` or an equivalent annotation to indicate that the method expects the value to be non-null.

If the variable is likely to be null, consider performing a null check before using the variable.

