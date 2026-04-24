# Class defines `clone` but does not inherit from `Cloneable`
**ID:** `JAVA-S0047` | **Lien:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-S0047)

![Major](https://img.shields.io/badge/severity-major-orange) ![Anti-pattern](https://img.shields.io/badge/type-anti_pattern-purple)

This class defines a `clone` method but it doesn't implement `Cloneable` .

This may lead to unexpected behavior of the class when cloned because the `clone` method may not rely on `Object.clone` or any superclass implementation of it to perform certain necessary operations.


## Examples

## Problematic Code

```java
// No "implements Cloneable" here
public class A {

    // No @Override here
    public A clone() {

        // ...

        return new A();
    }

}

class B extends A implements Cloneable {

    @Override
    public Object clone() {
        B newObj = (B)super.clone(); // This is an object of type A! This cast will fail with a ClassCastException.
        // ...

        return newObj;
    }
}
```

## Recommended
Make sure to implement `Cloneable` and call `super.clone` within the overridden `clone` method.


```java
public class A implements Cloneable {
    @Override
    public A clone() {
        A cloned = null;
        
        try {
            cloned = super.clone();
        } catch (CloneNotSupportedException e) {
            // ... handle the error
        }
        
        // ...

        return cloned;
    }
}
```

## Exceptions
There are some situations in which this is OK (e.g. you want to control how subclasses can clone themselves). Ensure that such a design is justified before you use it, and if so, document it well. Unexpected behavior may occur otherwise.

