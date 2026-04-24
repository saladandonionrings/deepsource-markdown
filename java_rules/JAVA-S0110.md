# `equals` method does not handle null valued operands
**ID:** `JAVA-S0110` | **Lien:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-S0110)

![Critical](https://img.shields.io/badge/severity-critical-red) ![Bug Risk](https://img.shields.io/badge/type-bug_risk-green)

This implementation of `equals` violates the contract defined by `java.lang.Object.equals` because it does not check for `null` being passed as the argument.

This can lead to the code throwing a `NullPointerException` when a null value is passed. This code violates the contract of `equals` because the receiver object ( `this` ) is always non null and so any null value passed is automatically not equal to `this` .


## Examples

## Problematic Code

```java
@Override
public boolean equals(Object o) {
    return this.field == o.field;
}

// ...

MyClass a = new MyClass(3);

a.equals(null); // Throws NullPointerException.
```

## Recommended

```java
@Override
public boolean equals(Object o) {
    return o != null && this.field == o.field;
}
```
All `equals` methods should return `false` if passed a null value. Assuming that the operands are always non-null may easily allow `NullPointerException` s to occur.

