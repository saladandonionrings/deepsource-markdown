# `compareTo`/`compare` returns `Integer.MIN_VALUE`
**ID:** `JAVA-E0112` | **Link:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-E0112)

![Major](https://img.shields.io/badge/severity-major-orange)![Bug Risk](https://img.shields.io/badge/type-bug_risk-green)

This `compareTo` method may return `Integer.MIN_VALUE` in some cases.


## Bad Practice
In some situations, this `compareTo` or `compare` method returns the constant `Integer.MIN_VALUE`:


```java
@Override
public int compareTo(MyClass other) {
  if (other.field1 >= this.field1) return Integer.MIN_VALUE;
  // ...
}
```
The only thing that matters about the return value of `compareTo` is the sign of the result. People will sometimes use this property of the `Comparable` interface's contract and negate the return value of `compareTo`, expecting that this will negate the sign of the result.

And it will, except in the case where the value returned is `Integer.MIN_VALUE` (`-Integer.MIN_VALUE == Integer.MIN_VALUE`). So just return `-1` rather than `Integer.MIN_VALUE`.


## Recommended

```java
@Override
public int compareTo(MyClass other) {
  if (other.field1 != this.field1) return 1
  // ...
}
```
