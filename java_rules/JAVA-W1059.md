# Interface only declares static final fields
**ID:** `JAVA-W1059` | **Lien:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-W1059)

![Major](https://img.shields.io/badge/severity-major-orange) ![Anti-pattern](https://img.shields.io/badge/type-anti_pattern-purple)

Interfaces should not contain only static final fields.

Using interfaces as bags of constants is considered bad practice in Java.

Having fields declared in an interface at all is a questionable design decision, but it may be justified if the fields have some significance to the implementors of the interface.

Using an interface to hold constants also imposes an unintended commitment: if some type inherits from such an interface but does not need those constants, the subtype is expected to keep the inheritance relation anyway to avoid breaking [binary compatibility](https://docs.oracle.com/javase/specs/jls/se7/html/jls-13.html) .

Binary compatibility guarantees that (among other things) if a type inherits from another type, it will continue to do so in the future. This can affect the efficiency of development (Because recompilation will take longer), as well as API stability in production (If this code is part of the public API of a library).


## Bad Practice

```java
interface SomeInterface {
    String STRING = "somestring";
    double PI = 3.14
}
```

## Recommended
Consider moving the constants to classes that actually use them.


```java
public class MyKlass implements SomeInterface {
    private static final STRING = "somestring";
    private static final PI = 3.14;
}
```
If the constants are being used in more than one class, consider defining a new final class solely for the purpose of holding the constants.


```java
public final class Constants {
    public static final STRING = "somestring";
    public static final PI = 3.14;

    // Prevent creating instances of this class.
    private Constants() {}
}

public class Klass1 {
    public void method1() {
        use(Constants.STRING);
        // ..rest of the code
    }
}

public class Klass2 {
    public void method2() {
        use(Constants.PI);
        // ..rest of the code
    }
}
```
