# `equals` method inherits parent class implementation instead of overriding `Object.equals`
**ID:** `JAVA-E0099` | **Link:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-E0099)

![Major](https://img.shields.io/badge/severity-major-orange)![Bug Risk](https://img.shields.io/badge/type-bug_risk-green)

This class defines an `equals` method that doesn't override `Object.equals(Object)`. In addition, it inherits an overridden `equals(Object)` method from a superclass. This may lead to unexpected results when comparing instances of this class.


## Bad Practice

```java
class Parent {

String id;

@Override
public boolean equals(Object other) { return other instanceOf Parent && this.id.equals(((Parent)other).id); }

}

class Child extends Parent {

String name;

public boolean equals(Child other) {
    return this.name == other.name && this.id == other.id;
}

}
```
Here, `Child` inherits an overridden definition of `equals` from `Parent` and also defines an overloaded version of `equals` taking a `Child` as an argument. Any standard library collection of `Child` will use `Parent`'s definition of `equals` instead of the intended overloaded version in `Child`, ignoring the `name` field completely.


## Recommended
It is recommended to explicitly override the default `equals(Object)` method to make things clear, and also prevent any subtle logic bugs if `equals` is required to behave differently in the child class.

By not doing so, equality checks for this class may no longer be symmetric or transitive.


```java
public boolean equals(Child other) {
    return this.name == other.name && this.id == other.id;
}

public boolean equals(Object other) {
    if (other instanceof Child) return this.equals((Child)other);
    else // ...
}
```
