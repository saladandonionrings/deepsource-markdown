# Static field accessed before being written
**ID:** `JAVA-W1060` | **Link:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-W1060)

![Major](https://img.shields.io/badge/severity-major-orange)![Bug Risk](https://img.shields.io/badge/type-bug_risk-green)

Static members should not be accessed before being written.

As per the [Java Language Specification](https://docs.oracle.com/javase/specs/jls/se7/html/jls-4.html#jls-4.12.5), every variable in a program must have a value before it is used. If a field (static or not) in some class is not initialized explicitly, the compiler will initialize it with a default value. With static initializers, this could become a problem.

A class is allowed to have multiple static initialization block. All static initialization blocks execute in the order they are declared in a source file. If a static initializer block accesses (reads) a static field that is assigned a value further down in some other static initializer block, such an access would evaluate to the default value that is assigned by the Java compiler. This might not be desired.


## Bad Practice

```java
class Klass {
    private static int value;

    static {
        if (value > 40) { // `value` is 0 here.
            // ...
        }
    }

    static {
        value = 10;
    }

}
```
The following snippet fails to compile; accessing `value` in the static block is an [incorrect forward reference](https://docs.oracle.com/javase/specs/jls/se7/html/jls-8.html#jls-8.3.2.3).


```java
class A {
    static {
        System.out.println(value); // This is a compile error.
    }

    private static int value = 10;
}
```

## Recommended
Consider initializing all the static fields.


```java
class Klass {
    private static int value = 10;

    static {
        access(value); // `value` is 10, as expected.
    }
}
```
Alternatively, rearrange your static blocks so that all static fields have a value before they are accessed.


```java
class Klass {
    private static int value;

    static {
        value = 10;
    }

    static {
        access(value); // `value` is 10 here.
    }
}
```
