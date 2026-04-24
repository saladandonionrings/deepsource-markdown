# Clone method does not invoke super method
**ID:** `JAVA-E0048` | **Lien:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-E0048)

![Critical](https://img.shields.io/badge/severity-critical-red) ![Bug Risk](https://img.shields.io/badge/type-bug_risk-green)

Non-final class defines a `clone` method that does not call `super.clone` .


## Bad Practice

```java
class T implements Cloneable {

    @Override
    public Object clone() {
        // Does not call super.clone();

        T newObj = new T(...);

        // ...

        return newObj;
    }

}

// ...

class U extends T implements Cloneable {

    @Override
    public Object clone() {
        U newObj = (U)super.clone(); // This is an object of type T! This cast will fail with a ClassCastException.
        // ...

        return newObj;
    }
}
```
If `T` is extended by a subclass `U` , and `U` calls `super.clone` , then it is likely that `U` 's `clone` method will get an object of type `T` . This will likely fail within the clone method itself when the subclass modifies data. Such code violates the standard contract for `clone` as stated by the [JavaDocs](https://docs.oracle.com/javase/7/docs/api/java/lang/Object.html#clone()) :

By convention, the returned object should be obtained by calling `super.clone` . If a class and all of its superclasses (except `Object` ) obey this convention, it will be the case that `x.clone().getClass() == x.getClass()` .


## Recommended
Always make sure to call `super.clone` when implementing `Cloneable` for any class.


```java
class T implements Cloneable {
    @Override
    public Object clone() {
        try {
            T newObj = super.clone();

            // ...

            return newObj;
        } catch (CloneNotSupportedException e) {
            // ...
        }
    }
}
```
