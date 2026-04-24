# @NoAllocation annotated methods should not create new objects
**ID:** `JAVA-E1059` | **Link:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-E1059)

![Major](https://img.shields.io/badge/severity-major-orange)![Bug Risk](https://img.shields.io/badge/type-bug_risk-green)

This method is annotated with the [`@NoAllocation`](http://javadox.com/com.google.errorprone/error_prone_core/2.3.4/com/google/errorprone/bugpatterns/NoAllocationChecker.html) annotation, indicating that new objects should not be created within it. However, it appears that this method either calls methods that allocate, or directly performs an operation such as string concatenation or an autoboxing operation that does perform an allocation.

Allocations can happen due to various reasons. An object could be created with the `new` keyword, two strings could be joined, resulting in a new `String`object being created, a lambda expression that captures local state could be created, or a primitive value could be automatically converted to a boxed value ([Autoboxing](https://docs.oracle.com/javase/tutorial/java/data/autoboxing.html)).

The `@NoAllocation` annotation can be used to indicate that such operations should be disallowed within a method annotated with it. Violating this contract may result in performance degradation due to increased load on the garbage collector, or may increase the chances of an out of memory scenario.


## Bad Practice

```java
@NoAllocation
void someAllocatingMethod(Character c) {
    // Some operation that may allocate new objects.
    Integer i = 34334; // values beyond [-128, 127] will not be cached by the JRE, a new Integer object will be created.
}


@NoAllocation
void someMethod() {

    // ...

    StringBuffer sb = new StringBuffer(); // Not correct!

    // This will cause a new Character object to be allocated due to the literal value being autoboxed.
    someAllocatingMethod('e');
}
```

## Recommended
Avoid performing any allocation operations within methods marked as `@NoAllocate`.

In particular, avoid the following operations within such a method:

* Calling methods not marked as`@NoAllocation`.* Passing primitive values to a generic method, or a method accepting a boxed value.* Assigning a primitive value to a variable of a boxed type (such as`Integer`,`Character`or`Double`).* Performing a string concatenation operation with non-constant strings (This will cause Java to allocate a`StringBuilder`to create the resultant string).
