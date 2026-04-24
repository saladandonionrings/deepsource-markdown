# Class doesn't override `equals` from superclass
**ID:** `JAVA-W0100` | **Link:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-W0100)

![Major](https://img.shields.io/badge/severity-major-orange)![Bug Risk](https://img.shields.io/badge/type-bug_risk-green)

This class extends a class that defines `equals` and adds fields, but doesn't define `equals` itself. Thus, equality on instances of this class will ignore its identity and its added fields.


## Bad Practice

```java
class Parent {

  int field1 = 3;

  @Override
  public boolean equals(Object other) {
    if (other is Parent && field1 == (Parent)other.field1)
      // ...
  }
}

class Child extends Parent {
  int field2 = 5;
}
```
Here, comparison of `Child` objects will use the `equals` method implemented in `Parent`.

Be sure this is what is intended, and that you don't need to override the `equals` method. Even if you don't need to override the `equals` method, consider overriding it anyway to document the fact that equality for the subclass works the same way as equality for the superclass.


## Recommended
Override the equals method in the child class even if you intend the current behavior; it will make the decision not to include the extra field in the equality condition explicit.


```java
class Child extends Parent {
  int field2 = 5;

  @Override
  public boolean equals(Object other) {
    return super.equals(other);
  }
}
```
