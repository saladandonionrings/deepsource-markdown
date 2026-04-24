# Class implements `Cloneable` but has not overridden the `clone` method
**ID:** `JAVA-S0046` | **Link:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-S0046)

![Major](https://img.shields.io/badge/severity-major-orange)![Bug Risk](https://img.shields.io/badge/type-bug_risk-green)

This class implements `Cloneable` but does not define or use the `clone` method. This may be because This is a violation of the `Cloneable` contract, as stated in the [JavaDocs](https://docs.oracle.com/javase/7/docs/api/java/lang/Cloneable.html):

By convention, classes that implement this interface should override `Object.clone` (which is protected) with a public method.

If other code attempts to clone an instance of this class, it may get an uninitialized or partially initialized version of the original object.


## Examples

## Problematic Code

```java
class SomeClass implements Cloneable {

    public Object field1 = new Object();

    @Override
    public Object clone() {
        SomeClass cloned = null;
        
        try {
            cloned = (SomeClass)super.clone();

        } catch (CloneNotSupportedException e) {
            // ...
        }

        cloned.field1 = new Object();

        return cloned;
    }
}

class SomeOtherClass extends SomeClass implements Cloneable {

    public Object field2 = new Object();

    // No implementation of clone...

}


// Elsewhere...

SomeOtherClass a = new SomeOtherClass();
a.field2 = 3;

// ...

SomeOtherClass b = (SomeOtherClass)a.clone();

a.field1 != b.field1; // True
a.field2 != b.field2; // false?! This should not be false! - a contract violation.
```

## Recommended
Define a `clone` method for the class to ensure that this does not occur:


```java
class SomeOtherClass extends SomeClass implements Cloneable {

    public Object field2;

    // ...

    @Override
    public Object clone() {
        SomeOtherClass other = null
        
        
        try {
            other = (SomeOtherClass)super.clone();

        } catch (CloneNotSupportedException e) {
            // ...
        }

        other.field2 = new Object();
        return other;
    }

}
```
