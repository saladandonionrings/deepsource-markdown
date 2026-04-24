# Invocation of `equals` on an array, which is equivalent to `==`
**ID:** `JAVA-E0348` | **Link:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-E0348)

![Major](https://img.shields.io/badge/severity-major-orange)![Bug Risk](https://img.shields.io/badge/type-bug_risk-green)

This method invokes the `.equals(Object o)` method on an array. Since arrays do not override the `equals` method of `Object`, calling `equals` on an array is the same as comparing their addresses.

To compare the contents of the arrays, use `java.util.Arrays.equals(Object[], Object[])`. To compare the addresses of the arrays, it would be less confusing to explicitly check pointer equality using `==`.


## Bad Practice

```java
String[] a = new String[10]();
String[] b = new String[10]();

// a and b are populated with the same data.

assertTrue(a.equals(b)); // Fails, address of a not equal to address of b.

// Or

assertTrue(a == b); // Fails for the same reason.
```

## Recommended
Use `java.util.Arrays.equals` instead.


```java
assertTrue(Arrays.equals(a, b)); // Compares contents of a and b.
```
