# Do not call hashCode directly on an array class
**ID:** `JAVA-E1044` | **Lien:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-E1044)

![Major](https://img.shields.io/badge/severity-major-orange) ![Bug Risk](https://img.shields.io/badge/type-bug_risk-green)

Calling `T[].hashCode()` (where `T` is some class) will not take into account the contents of the array.

Use the [ `Arrays.hashCode(Object[])` ](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/util/Arrays.html#hashCode(java.lang.Object%5B%5D)) static method to calculate an array's hash code instead.

Any array type inherits the default `equals()` and `hashCode()` implementations from `Object` . By default, object equality works by comparing reference addresses (like `a == b` ), and an object's hash code can also similarly depend on its address (The JVM specification leaves this at the discretion of the implementation). Make sure this is the behavior you actually want.


## Bad Practice

```java
String[] names = ...;

int namesHash = names.hashCode(); // Bad.
```

## Recommended

```java
int namesHash = Arrays.hashCode(names);
```

## Exceptions
If you actually intend to use the default `hashCode` implementation of arrays, you may safely ignore this issue.

