# JUnit5 test classes and methods should be package-private
**ID:** `JAVA-W1058` | **Lien:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-W1058)

![Minor](https://img.shields.io/badge/severity-minor-yellow) ![Anti-pattern](https://img.shields.io/badge/type-anti_pattern-purple)

JUnit5 test classes and methods should be package-private.

Unlike JUnit4 which required all the test classes and methods to be declared `public` , in JUnit5 they can be anything but `private` . To enforce maximum encapsulation, it is recommended to declare test classes and methods as package-private.


## Bad Practice

```java
public class MyTest {
    @Test
    public void testThis() {
        // ..test things
    }
}
```

## Recommended
Consider making your test classes and methods package-private.


```java
class MyTest {
    @Test
    void testThis() {
        // ..test things
    }
}
```
