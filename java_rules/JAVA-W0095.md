# `equals` checks for incompatible operand
**ID:** `JAVA-W0095` | **Lien:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-W0095)

![Major](https://img.shields.io/badge/severity-major-orange) ![Anti-pattern](https://img.shields.io/badge/type-anti_pattern-purple)

This `equals` method is checking to see if the argument is some incompatible type (i.e., a class that is neither a supertype nor subtype of the class that defines the `equals` method).


## Bad Practice
This class might have an `equals` method that looks like:


```java
public boolean equals(Object o) {
    if (o instanceof Foo)
        return name.equals(((Foo)o).name);
    else if (o instanceof String) // Foo is not related to String in any way!
        return name.equals(o);
    else return false;
}
```
This is considered bad practice, as the `equals` method will not be symmetric and transitive. Without those properties, very unexpected behaviors are possible, such as the scenario of `a equals b AND b equals c IMPLIES a equals c` being false.


## Recommended
Do not perform checks for unrelated types in the equals method.


## Exceptions
In some cases, such as when implementing a comparator for elements of a collection, it may be useful to define an `equals` method which checks for an entirely different type to allow for greater flexibility. If you have such a requirement, it should be safe to use this pattern to achieve it.

However, do note that while Java's collection APIs use the `equals` method of the object we are looking for and not that of the objects within the collection, they are not obligated by contract to keep this behavior and could cause problems in a future update.

