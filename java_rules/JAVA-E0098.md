# `equals` method is overloaded but not overridden
**ID:** `JAVA-E0098` | **Link:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-E0098)

![Major](https://img.shields.io/badge/severity-major-orange)![Bug Risk](https://img.shields.io/badge/type-bug_risk-green)

This class defines a version of the `equals` method which does not take an `Object` as its parameter, meaning it overloads the original `Object.equals(Object)` method instead of overriding it. Standard library collections will behave unexpectedly when handling values of this type.


## Bad Practice

```java
class MyObj {

    int val;

    public MyObj(int val) {
        this.val = val;
    }

    // Parameter is of type MyObj, not Object.
    public boolean equals(MyObj other) {
        // ...
    }
}
```
This can lead to subtle logic errors, because other code, such as standard library containers may use only the default `equals(Object)` function and not the class specific overload. For example, the following code will not work as expected:


```java
List<MyObj> myObjs = Arrays.asList(new MyObj(42));

assert(myObjs.contains(new MyObj(42)) == true); // Error!
```
It seems obvious that `myObjs` contains the same value, and that the above code should have run successfully. However, `List.contains` will not take the overloaded `equals` method into account, and will use the default `java.lang.Object.equals` implementation which simply compares reference addresses instead.


## Recommended
There are rarely any cases where an overloaded `equals` method alone has any advantages over just overriding the default implementation. Just override `equals(Object)` instead. If you believe it is better to have an overload specific to a particular class, consider override `equals(Object)` and calling your overriding method from it to ensure that `equals` always behaves predictably.


```java
public boolean equals(MyObj other) {
    // ...
}


public boolean equals(Object other) {

    if (other instanceof MyObj) return this.equals((MyObj) other);
    else // ...
}
```
