# Classes that contain only static members should not be instantiated
**ID:** `JAVA-W1035` | **Link:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-W1035)

![Major](https://img.shields.io/badge/severity-major-orange)![Anti-pattern](https://img.shields.io/badge/type-anti_pattern-purple)

This code seems to be creating an instance of a class with only static members. Such a class does not need to be instantiated, since all members can be accessed with just the class itself.


## Bad Practice

```java
final class StaticHolder {
    public final static Object THING1 = new Object();
    public final static Object THING2 = new Object();
}

// ... elsewhere ...

StaticHolder someHolder = new StaticHolder();

// OR

Object thing = new StaticHolder().THING1; // Unnecessary!
```

## Recommended
Use the class instance directly.


```java
Object thing = StaticHolder.THING1;
```
