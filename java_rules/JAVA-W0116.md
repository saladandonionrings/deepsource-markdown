# Hashcode must be implemented along with Equals
**ID:** `JAVA-W0116` | **Lien:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-W0116)

![Minor](https://img.shields.io/badge/severity-minor-yellow) ![Bug Risk](https://img.shields.io/badge/type-bug_risk-green)

In Java, `Object.hashCode()` behaves similarly to `Object.equals()` ; if two objects are equal, they are guaranteed to have the same hash code.

This class appears to define an equality condition but uses the `hashCode` implementation defined in its parent class. This could result in two objects that appear to be equal but do not have the same hash code; a violation of the contract. In the more common case, this would result in a greater chance of two objects having the same hash code, which reduces the performance of collections like `HashMap` .

From [Oracle's Java documentation for `java.lang.Object` ](https://docs.oracle.com/javase/7/docs/api/java/lang/Object.html#hashCode()) :

The general contract of `hashCode` is:

* Whenever it is invoked on the same object more than once during an execution of a Java application, the hashCode method must consistently return the same integer, provided no information used in equals comparisons on the object is modified. This integer need not remain consistent from one execution of an application to another execution of the same application.
* If two objects are equal according to the equals(Object) method, then calling the hashCode method on each of the two objects must produce the same integer result.
* It is not required that if two objects are unequal according to the equals(java.lang.Object) method, then calling the hashCode method on each of the two objects must produce distinct integer results. However, the programmer should be aware that producing distinct integer results for unequal objects may improve the performance of hash tables.


## SubClassad Practice

```java
class ParentClass {

    int iValue;
    String sValue = "abc";

    @Override
    public int hashCode() {
        // We generate the hash code for ParentClass using the hash code values of its member fields.
        int res = iValue.hashCode();
        res = 31 * res + sValue.hashCode();
        return res;
    }

    @Override
    public boolean equals(Object other) {
        return other instanceof ParentClass && iValue == other.iValue && sValue.equals(other.sValue);
    }
}

// SubClass does not implement hashCode:
class SubClass extends ParentClass {
    UUID uuid = UUID.random();

    // ...

    // In our overridden implementation of equals, we only consider the uuid in the equality check!
    @Override
    public boolean equals(Object other) {
        return other instanceof SubClass && this.uuid.equals(other.uuid);
    }
}
```
When both values are of type `ParentClass` , things are simple:


```java
ParentClass a = new ParentClass();
a.iValue = 3;

ParentClass b = new ParentClass();
b.iValue = 3;

a.hashCode(); // -995480412
b.hashCode(); // -995480412
a.equals(b); // true
```
Consider what happens when we try to generate the hashcode of two instances of `SubClass` with the same `uuid` value, but different values of `iValue` and/or `sValue`


```java
SubClass c = new SubClass();
c.iValue = 4;

SubClass d = new SubClass();
d.iValue = 3;
d.id = c.id;

c.hashCode(); // -995480381
d.hashCode(); // -995480412 !!! The hash codes are not the same.
c.equals(d); // true !!!
```
Note that even though the equality condition of `SubClass` is fulfilled by `c` and `d` , the hash codes of both are different. This could have been avoided by overriding `hashCode` for `SubClass` as well.


## Recommended
Overriding `hashCode()` along with `equals()` can help in avoiding such gaps in logic.


```java
@Override
public int hashCode() {
    // ...
}
```
Even if the `hashCode` implementation is intended to be the same as the parent implementation, this may be made clearer by overriding and delegating to the super implementation of `hashCode` .

