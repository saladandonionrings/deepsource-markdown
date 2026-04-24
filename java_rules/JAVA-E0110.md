# `equals` method does not handle null valued operands
**ID:** `JAVA-E0110` | **Lien:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-E0110)

![Critical](https://img.shields.io/badge/severity-critical-red) ![Bug Risk](https://img.shields.io/badge/type-bug_risk-green)

This implementation of `equals(Object)` violates the contract defined by `java.lang.Object.equals(Object)` because it does not check for `null` being passed as the argument.

`equals` must always return `false` if its argument is `null` .

This can lead to the code throwing a `NullPointerException` when a null value is passed. One property of any non-static method in Java is that the receiver object ( `this` ) is always non-null. This code violates the contract of `equals` because any null value passed is automatically not equal to `this` .


## Bad Practice

```java
@Override
public boolean equals(Object o) {
    return this.field == o.field;
}

// ...

MyClass a = new MyClass(3);

a.equals(null); // Throws a NullPointerException.
```

## Recommended

```java
@Override
public boolean equals(Object o) {
    return o != null && this.field == o.field;
}
```
The `equals` method should return `false` if passed a null value. Assuming that the operands are always non-null may easily allow `NullPointerException` s to occur.

