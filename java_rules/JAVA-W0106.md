# `equals` always returns `true`
**ID:** `JAVA-W0106` | **Lien:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-W0106)

![Major](https://img.shields.io/badge/severity-major-orange) ![Anti-pattern](https://img.shields.io/badge/type-anti_pattern-purple)

This class defines an `equals` method that always returns `true` . This is imaginative, but will break a lot of things.


## Bad Practice
Always returning true breaks the symmetry of equality for objects of `MyClass` .


```java
class MyClass {

    private int priv = 0;

    @Override
    public boolean equals(Object other) { return true; }

}

Integer a = 2;
MyClass b = new MyClass();


assert(a.equals(b) == false); // Correct behavior.
assert(b.equals(a) == false); // Not symmetric.
```

## Recommended
Redefine the `equals` method to return a more sensible value.


```java
class MyClass {

    private int priv = 0;

    @Override
    public boolean equals(Object other) {
        return other instanceof MyClass && priv == other.priv;
    }

}
```
