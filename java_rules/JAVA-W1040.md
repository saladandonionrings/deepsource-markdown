# Local variable is assigned null value and not read after
**ID:** `JAVA-W1040` | **Lien:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-W1040)

![Minor](https://img.shields.io/badge/severity-minor-yellow) ![Anti-pattern](https://img.shields.io/badge/type-anti_pattern-purple)

A local variable is assigned `null` , and no reads of that variable occur after this assignment.

Such an assignment may have been done to help along the garbage collector, but this has been unnecessary since Java 6, where the garbage collector got [a lot smarter](https://www.oracle.com/java/technologies/javase/6u14.html) .

Current JVMs are likely to optimize away assignments that are not used, meaning there is no net difference.


## Bad Practices

```java
String username = ...;

// ...

username = null;

// username is not used again.
```
In the example above, the final assignment to `username` is not followed by any other operation.


## Recommended
If this assignment was only used to set a final `null` value, consider removing it.

