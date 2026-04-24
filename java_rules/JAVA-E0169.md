# Classes should not have the same name as any of their superclasses or implemented interfaces
**ID:** `JAVA-E0169` | **Link:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-E0169)

![Major](https://img.shields.io/badge/severity-major-orange)![Anti-pattern](https://img.shields.io/badge/type-anti_pattern-purple)

This class/interface has a name that is identical to that of an implemented/extended class or interface, except that the supertype is in a different package (e.g., `alpha.Foo` extends `beta.Foo`).

Consider the following 3 classes:


```java
package com.example.A;

public class Foo {

    // ...

}

// different file, different package

package com.example.B;

import com.example.A.Foo;

public class Bar extends Foo {

    // ...

}

// different file, same package as Bar

package com.example.B;

public class Foo extends Bar {

    // ...

}
```
The example given above is perfectly legal, albeit potentially confusing. Because the two classes named `Foo` are separated by the package hierarchy, no name clashes occur. However, any reader who does not already know this codebase could be confused as to where a class, method or field came from.

This could lead to accidental overloading of a method due to lack of knowledge.

Avoid giving multiple classes the same name, even across packages.

