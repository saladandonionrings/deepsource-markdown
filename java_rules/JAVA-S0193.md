# Class is Serializable, but doesn't define `serialVersionUID`
**ID:** `JAVA-S0193` | **Lien:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-S0193)

![Minor](https://img.shields.io/badge/severity-minor-yellow) ![Anti-pattern](https://img.shields.io/badge/type-anti_pattern-purple)

This class implements the `Serializable` interface, but does not define a `serialVersionUID` field.

A change as simple as adding a reference to a `.class` object will add synthetic fields to the class, which will unfortunately change the implicit `serialVersionUID` (e.g., adding a reference to `String.class` will generate a static field `class$java$lang$String` ). Also, different source code to bytecode compilers may use different naming conventions for synthetic variables generated for references to class objects or inner classes.

To ensure interoperability of Serializable across versions, consider adding an explicit `serialVersionUID` .

