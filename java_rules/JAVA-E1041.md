# Interface is unimplementable
**ID:** `JAVA-E1041` | **Link:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-E1041)

![Major](https://img.shields.io/badge/severity-major-orange)![Bug Risk](https://img.shields.io/badge/type-bug_risk-green)

This interface cannot be implemented because it declares one or more methods that clash with the names of methods defined in `java.lang.Object`.

Rename the methods that clash with `Object`'s declared methods so that there are no more conflicts.

The methods defined in `Object` have certain specific properties:

* Attempting to redeclare`Object`'s`final`methods in a class or interface with the same name and formal parameters will always fail with a compilation error.* Public overridable methods of`Object`(such as`toString()`) will similarly not allow overloads that have the same argument types but different return types.* Protected methods, such as`clone()`may be defined with a different return type in an interface without incident, but cannot ever be implemented, because the declarations present in both the interface and within`Object`would clash.
The third point in particular is not immediately apparent. But when a class attempts to inherit from an affected interface, `javac` will raise a compile error.


## Bad Practice

```java
interface SomeInterface {

    int toString(); // This is a compile error.

    void wait(); // wait is final in object, it cannot be overridden.

    String clone(); // clone is protected in Object and will clash when this interface is implemented.
}

class SomeClass implements SomeInterface {


    @Override
    String clone() { // This method's signature will clash with the one defined in Object.
        return "";
    }
}
```

## Recommended
Give the offending method(s) a different name to avoid clashes with `Object`'s members.

