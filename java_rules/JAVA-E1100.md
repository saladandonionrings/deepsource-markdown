# Methods should not have different nullability than their super methods
**ID:** `JAVA-E1100` | **Link:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-E1100)

![Major](https://img.shields.io/badge/severity-major-orange)![Bug Risk](https://img.shields.io/badge/type-bug_risk-green)

If a method of a superclass has one particular nullability annotation applied to it, avoid marking any overrides in subtypes with a different nullability annotation.

Make sure to use the same annotation present on the super method as much as possible.

This issue is raised when the parent method is annotated with a particular nullability annotation, and the child method is not, or is annotated with a different annotation than the parent method.

This issue will also be raised on parameters of such overloaded methods that may have differing, or no nullability annotations as well.


## Bad Practice
Consider this code, where class `B` extends class `A`, and overrides `A.a` with `@Nullable` instead of `@Nonnull` as `A.a` has been marked.


```java
class A {

    @Nonnull
    Integer a(int x) {
        return x;
    }
}

class B extends A {

    @Nullable
    @Override
    Integer a(int x) {
        if (x % 2 != 0) return x;
        return null;
    }
}
```
This would cause issues if the classes in question were used with polymorphism, like in the following code:


```java
A someInstance = new A();

int someInt = someInstance.a(3); // works.

someInstance = new B(); // An instance of `B` is assigned to a variable of type `A`.

someInt = someInstance.a(4); // Throws an NPE!
```

## Recommended
Use consistent nullability annotations in overriding methods, and avoid changing the method contract specified by the parent class as much as possible.

If you need to have entirely new behavior with different constraints, create an overload, or a new method entirely.


```java
class B extends A {

    @Nullable
    Integer aNullable(int x) {
        if (x % 2 != 0) return x;
        return null;
    }
}
```
