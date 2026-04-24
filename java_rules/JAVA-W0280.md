# References should not be compared with `==`/`!=`
**ID:** `JAVA-W0280` | **Link:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-W0280)

![Major](https://img.shields.io/badge/severity-major-orange)![Bug Risk](https://img.shields.io/badge/type-bug_risk-green)

Two references are compared with the `==`/`!=` operators. This may not work as expected and may cause bugs due to wrong assumptions.

There are two ways to compare objects, or "reference types" in general; by value, or by reference. The `==`/`!=` operators only allow for comparison by reference. This means that the only criterion for equality via these operators is if the two references point to the same object.

If you want to compare objects by value, you must use the `equals` method to do so, as it can be overridden to provide class specific equality checking. The reason for this is that it is possible to create distinct instances that are equal by value but do not compare as equal with `==` because they are not the same object.


## Bad Practice

```java
String a = new String("abc");

String b = new String("abc");

a == b; // false

a.equals(b); // true
```

## Recommended
Note that the default implementation of `equals` defined in `Object` is the same as that of `==`. Unless you override `equals` and add checks for your class's fields, `equals` will behave similarly to `==`.


```java
public class ThisClass {
    @Override
    public boolean equals(Object other) {
        return other instanceof ThisClass && this.a == other.a; // you can define your equality criteria here.
    }

}
```

## Exceptions
It is possible that in some cases this issue may be a false positive. This can occur if the logic explicitly relies on reference comparison instead of object equality. In such cases it is safe to mark the offending line with a `skipcq` comment:


```java
if (a != b) { // skipcq JAVA-S0280
    // ...
```
