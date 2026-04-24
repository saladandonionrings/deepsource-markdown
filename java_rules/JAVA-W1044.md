# `instanceof` check with a known null value will always return false
**ID:** `JAVA-W1044` | **Lien:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-W1044)

![Major](https://img.shields.io/badge/severity-major-orange) ![Anti-pattern](https://img.shields.io/badge/type-anti_pattern-purple)

This `instanceof` test will always return false, since the value being checked is guaranteed to be `null` .

Although this is safe, make sure it isn't an indication of some misunderstanding or some other logical error.

Java's `instanceof` operator will return false if given a `null` value.


## Bad Practice

```java
String alwaysNull = null;

// ...

if (alwaysNull is CharSequence) { // will never be true!

}
```

## Recommended
Check if there is some missing operation that should be performed before the `instanceof` check.

