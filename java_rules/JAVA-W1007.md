# Fields must not shadow other fields with the same name from super classes
**ID:** `JAVA-W1007` | **Lien:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-W1007)

![Major](https://img.shields.io/badge/severity-major-orange) ![Bug Risk](https://img.shields.io/badge/type-bug_risk-green)

This field appears to have the same name as a field in a super class.

This will prevent the superclass field from being accessed, and you will only be able to access the new field.

Rename the new field to avoid conflicts.


## Bad Practice

```java
public class A {
    public String firstValue;

    public int secondValue;
}

public class B extends A {
    public boolean firstValue; // Wasn't firstValue a string before?
}
```

## Recommended

```java
public class A {
    public String firstValue;

    public int secondValue;
}

public class B extends A {
    public boolean thirdValue;
}
```

## Exceptions
This issue will not be raised for static fields, or when the parent class field is private.

