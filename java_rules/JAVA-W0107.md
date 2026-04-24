# `equals` always returns `false`
**ID:** `JAVA-W0107` | **Link:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-W0107)

![Major](https://img.shields.io/badge/severity-major-orange)![Anti-pattern](https://img.shields.io/badge/type-anti_pattern-purple)

This class defines an `equals` method that always returns false. This means that an object is not equal to itself, and it is impossible to create useful `Map`s or `Set`s of this class.

More fundamentally, it means that `equals` is not reflexive, one of the requirements of its API.


## Equals

## Bad Practice

```java
class MyClass {

    @Override
    public boolean equals(Object o) {
        return false;
    }

}
```

## Recommended

```java
class MyClass {

    @Override
    public boolean equals(Object o) {
        return ...; // return a value based on required criteria for this object.
    }

}
```
This may be an attempt to reset the semantics of equality for this class; meaning objects of this class will be compared in the same way as those of the `Object` class. If you need to override an `equals` method inherited from a different superclass in such a way, you could use something like this:


```java
@Override
public boolean equals(Object o) {
    return this == o;
}
```
