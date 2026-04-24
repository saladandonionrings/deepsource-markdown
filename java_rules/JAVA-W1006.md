# Method and field names must be dissimilar
**ID:** `JAVA-W1006` | **Lien:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-W1006)

![Minor](https://img.shields.io/badge/severity-minor-yellow) ![Anti-pattern](https://img.shields.io/badge/type-anti_pattern-purple)

This class's method names are the same as its field names, or differ from the field names only in terms of capitalization. This will cause confusion when reading the code, and will also decrease readability.

Rename the similar fields or methods so that their names are more readily distinguishable.


## Bad Practice

```java
class SomeClass {
    private String name;
    private int age;

    // This method seems to be a getter for the age field, but it does not return the value of age...
    public int age() {
        return 5;
    }

    // This could be mistaken for the age field's getter...
    public String Age() {
        return "stone";
    }

    private boolean isSomething;

    // can be confusing. At first glance, this may be mistaken to be a getter, but it is actually a setter!
    public void something(boolean newVal) {
        isSomething = newVal;
    }

    public void doIt() {
        // ...
    }
}

class SomeChildClass extends SomeClass {
    // What does this string represent?
    // There are already two methods and one field with names similar to this one.
    String Age;

    // This method may be an overload for the doIt method in the parent class,
    // but it is spelled differently than the parent method... was this intentional?
    public String doit(String arg1, int arg2) {
        // ...
    }
}
```

## Recommended
Name fields based on the value they represent. Never name a field with the same name as a different field or method.

Name methods based on the actions they are meant to perform, and ensure that their names can't be mistaken for that of a different method or field, unless you are actually attempting to overload something else.


```java
class SomeClass {

    private String name;
    private int age;

    String getName() {
        return this.name;
    }

    int getAge() {
        return this.age;
    }

    public void doIt() {
        // ...
    }
}

class SomeChildClass extends SomeClass {
    // An overload of the parent method.
    public void doIt(int val1, int val2) {
        // ...
    }

    // An override of the parent method.
    @Override
    public void doIt() {
        // ...
    }
}
```
