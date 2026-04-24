# Private field is never initialized
**ID:** `JAVA-E1065` | **Lien:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-E1065)

![Critical](https://img.shields.io/badge/severity-critical-red) ![Bug Risk](https://img.shields.io/badge/type-bug_risk-green)

This private field is never initialized before use. This may cause improper behavior at runtime, or even a `NullPointerException` .

Check if the logic that uses the field is correct; add an initializer to the declaration or initialize the field at an appropriate point before use.

If a field is not explicitly initialized, Java will set the value of the field to a default value at runtime. This default value depends on the type:

* For primitives, it is `0` (or the floating point equivalent)
* For descendants of `Object` , the default value is `null` .

Java does not check if a field is properly initialized in the way it checks local variables, and this can easily prevent one from immediately noticing that something is wrong.


## Bad Practice

```java
// Never initialized, never assigned a value.
private String internalField;

String someMethod() {
    someInternalCode(internalField); // `internalField` will be null!
}
```

## Recommended
Assign a valid default to the field, or initialize it wherever sensible.


```java
private String internalField = "defaultValue";

// ...
```

## Exceptions
This issue will not be reported for fields marked as being injected (marked with annotations such as `@Inject` or `@Autowired` ).

