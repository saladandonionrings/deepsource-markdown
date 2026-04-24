# `equals` method defined for enumeration
**ID:** `JAVA-E0096` | **Link:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-E0096)

![Major](https://img.shields.io/badge/severity-major-orange)![Bug Risk](https://img.shields.io/badge/type-bug_risk-green)

This `enum` defines an overload for the `equals` method using the `enum`'s own class type. The `equals()` method of an `enum` is not meant to be overloaded (or overridden), and doing so may cause weird bugs to crop up when values of this `enum` are compared.

Equality on enumerations is defined using object identity, that is, the address of the object. This works because each variant of an `enum` is essentially a static final instance of that `enum` class with data corresponding to the declared variant. All usages of an `enum's variants point to the same set of static constants.

Overloading the equals method for an `enum` value is exceptionally bad practice since the more specialized overloaded method will always be chosen when comparing `enum` variants against each other. Unless the `enum` value is cast to `Object` before comparison, the original `equals` method will not be used.


## Bad Practice

```java
enum TestEnum {
    First("st"),
    Second("nd"),
    Third("rd"),
    Fourth("th"),
    Fifth("th"),
    Sixth("th"),
    Seventh("th"),
    Eighth("th"),
    Ninth("th");

    String notation;

    TestEnum(String n) {
        notation = n;
    }

    // Overload of equals(Object).
    public boolean equals(TestEnum other) {
        return this.notation == other.notation;
    }
}

// ...

assertTrue(TestEnum.Sixth.equals((Object)TestEnum.Ninth));  // False...
assertTrue(TestEnum.Sixth.equals(TestEnum.Ninth));          // True?!
```

## Recommended
Remove the unnecessary `equals` method.


```java
enum TestEnum {
    First("st"),
    Second("nd"),
    Third("rd"),
    Fourth("th"),
    Fifth("th"),
    Sixth("th"),
    Seventh("th"),
    Eighth("th"),
    Ninth("th");

    String notation;

    TestEnum(String n) {
        notation = n;
    }
}
```
