# Array elements should match the runtime type of the array
**ID:** `JAVA-E1079` | **Link:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-E1079)

![Major](https://img.shields.io/badge/severity-major-orange)![Bug Risk](https://img.shields.io/badge/type-bug_risk-green)

This array element assignment seems to have a different type than what the array was initialized with. Performing this operation will trigger an [ArrayStoreException](https://docs.oracle.com/javase/8/docs/api/java/lang/ArrayStoreException.html) at runtime.


## Bad Practice

```java
Number[] array = new Integer[10];

// Elsewhere...

array[4] = new BigDecimal(3.2);
```

## Recommended
Change the type of the array, or the assigned value to ensure that there is no incompatibility:


```java
// The runtime type now matches the declared type.
Number[] array = new Number[10];

// Elsewhere...

array[4] = new BigDecimal(3.2);


// OR

Integer[] array = new Integer[10];

// Elsewhere...

// This fails at compile time now.
// array[4] = new BigDecimal(3.2);

array[4] = 34;
```
