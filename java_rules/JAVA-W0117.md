# Class overrides `hashCode` but not `equals`
**ID:** `JAVA-W0117` | **Link:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-W0117)

![Minor](https://img.shields.io/badge/severity-minor-yellow)![Bug Risk](https://img.shields.io/badge/type-bug_risk-green)

This class defines a `hashCode` method but inherits its `equals` method from `java.lang.Object` (which defines equality by comparing object references). Although this will probably satisfy the contract that equal objects must have equal hashcodes, it is probably not what was intended by overriding the `hashCode` method.

Overriding `hashCode` implies that the object's identity is based on criteria more complicated than simple reference equality. If the accompanying `equals` implementation does not follow similar criteria as the `hashCode` implementation, situations where two objects may compare as equal but may not have the same hashCode may arise.

Note that while it is required by contract that two objects which compare as equal also have the same hashCode values, it is not required for both objects to have different hashCodes when they are **not** equal.


## Bad Practice

```java
class SomeClass {

    /* fields */

    @Override
    int hashCode() {
        // ... hashCode computation ...
        return computedHashcode;
    }

    // no equals implementation.
}
```

## Recommended
Override the `equals` method as well to explicitly specify conditions for equality.


```java
class SomeClass {

    /* fields */

    @Override
    int hashCode() {
        // ... hashCode computation ...
        return computedHashcode;
    }

    @Override
    boolean equals() {
        // equality condition
    }
}
```
If you don't think instances of this class will ever be inserted into a HashMap/HashTable, the recommended `hashCode` implementation to use is:


```java
public int hashCode() {
    throw new NotImplementedException("hashCode not designed");
    return 42; // any arbitrary constant will do
}
```
