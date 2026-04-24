# `serialVersionUID` should be correctly declared
**ID:** `JAVA-E1042` | **Link:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-E1042)

![Major](https://img.shields.io/badge/severity-major-orange)![Bug Risk](https://img.shields.io/badge/type-bug_risk-green)

The `serialVersionUID` field must be declared as `<access modifier> static final long serialVersionUID`. Not declaring it as such will prevent Java from processing it.

The `serialVersionUID` field has significance when using Java's native serialization API. When declared on a `Serializable` class, it will be used by Java as a tool to verify proper serialization and deserialization. If it is declared incorrectly, Java will instead opt to generate a `serialVersionUID` automatically based on the contents of the class.

Such automatic generation can be problematic for certain scenarios. For example, compiling the source with different JDKs may yield different values of `serialVersionUID` for the same class. If the UID of the serialized data does not match the UID as present in the class loaded in the JVM, an [`InvalidClassException`](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/io/InvalidClassException.html) will be thrown.


## Bad Practice

```java
public static int serialVersionUID = 3; // Wrong.
```

## Recommended
Declare `serialVersionUID` with this specific signature (only the visibility and the value of the field may be changed).


```java
public static final long serialVersionUID = 3L; // Correct.
```
