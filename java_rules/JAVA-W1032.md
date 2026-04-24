# Floating point values should not be compared with relational operators in comparison methods
**ID:** `JAVA-W1032` | **Lien:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-W1032)

![Minor](https://img.shields.io/badge/severity-minor-yellow) ![Anti-pattern](https://img.shields.io/badge/type-anti_pattern-purple)

A comparison method (such as `compareTo` , `equals` or `compare` ) seems to be using relational operators ( `<` , `>` , et al) to compare floating point numbers. The behavior of these operators deviates from the method contracts of `Float.compare` and `Double.compare` , which may cause inconsistent behavior with standard library collections, and possibly other container APIs as well.

In Java, the way relational operators ( `<` , `>` , et al) evaluate floating point values differs from how `Float.compare()` or `Double.compare()` are implemented.

From the JavaDoc for [ `Float.compare(float, float)` ](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/lang/Float.html#compare(float,float)) :

There are two ways in which comparisons performed by this method differ from those performed by the Java language numerical comparison operators ( `<` , `<=` , `==` , `>=` , `>` ) when applied to primitive `float` values:

* `Float.NaN` is considered by this method to be equal to itself and greater than all other `float` values (including `Float.POSITIVE_INFINITY` ).
* `0.0f` is considered by this method to be greater than `-0.0f` .

Similar differences exist for `Double` , and these differences also apply to the `equals` and `compare` methods of these types.


## Bad Practice

```java
private double someDoubleField;

@Override
int compareTo(T other) {
    if (other.someDoubleField == this.someDoubleField) {
        // ...
    }
}
```

## Recommended
Use the relevant `compareTo` or `compare` method on the floating point values to be compared instead.


```java
@Override
int compareTo(T other) {
    if (Double.compare(this.someDoubleField, other.someDoubleField)) {
        // ...
    }
}
```

## Exceptions
This issue is only reported when such comparisons are found within comparison methods such as `compareTo` and `equals` .

