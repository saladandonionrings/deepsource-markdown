# Class overrides `compareTo()` but not `equals()`
**ID:** `JAVA-W1056` | **Lien:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-W1056)

![Major](https://img.shields.io/badge/severity-major-orange) ![Bug Risk](https://img.shields.io/badge/type-bug_risk-green)

This class implements `Comparable<T>` and overrides `compareTo()` , but it does not override `equals()` so that the implementations of `compareTo` and `equals` are in sync.

This will cause issues when performing comparison/equality checks, and may cause inconsistent behaviour when collections of this class are sorted.

Make sure to add a corresponding `equals()` implementation which agrees with `compareTo()` .

This class defines a `compareTo(...)` method but inherits its `equals()` method from `java.lang.Object` . Generally, the value of `compareTo()` should return zero if and only if `equals()` returns true.

From the JavaDoc for [ `Comparable<T>.compareTo()` ](https://docs.oracle.com/en/java/javase/17/docs/api/java.base/java/lang/Comparable.html#compareTo(T)) :

It is strongly recommended, but not strictly required that `(x.compareTo(y)==0) == (x.equals(y))` . Generally speaking, any class that implements the `Comparable` interface and violates this condition should clearly indicate this fact. The recommended language is:

Note: this class has a natural ordering that is inconsistent with `equals` .


## Bad Practice

```java
class OnlyCompareTo implements Comparable<OnlyCompareTo> {
    int field1 = 0;

    @Override
    int compareTo(OnlyCompareTo other) {
        if (other == null) return 1;
        return Integer.compare(field1, other.field1);
    }
}
```

## Recommended
Consider implementing an `equals` method that matches the behavior of the defined `compareTo` method.


```java
class CompareToAndEquals implements Comparable<CompareToAndEquals> {
    int field1 = 0;

    @Override
    int compareTo(CompareToAndEquals other) {
        if (other == null) return 1;
        return Integer.compare(field1, other.field1);
    }

    @Override
    boolean equals(Object other) {
        return this == other || (other != null && other.field1 == this.field1);
    }
}
```
