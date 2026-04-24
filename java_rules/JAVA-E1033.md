# Custom serialization method is declared with an incorrect signature
**ID:** `JAVA-E1033` | **Link:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-E1033)

![Major](https://img.shields.io/badge/severity-major-orange)![Bug Risk](https://img.shields.io/badge/type-bug_risk-green)

This class declares one or more custom serialization methods but these methods do not match the signatures expected by Java's serialization API.

Change the signature(s) to match the expected type.

Java expects the signatures of the `readObject`, `readObjectNoData` and `writeObject` methods to exactly match certain signatures, as codified in the specification for the [`Serializable` API](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/io/Serializable.html):

Classes that require special handling during the serialization and deserialization process must implement special methods with these exact signatures:


```java
private void writeObject(java.io.ObjectOutputStream out) throws IOException;

private void readObject(java.io.ObjectInputStream in) throws IOException, ClassNotFoundException;

private void readObjectNoData() throws ObjectStreamException;
```
If these methods are not declared with the correct signatures, Java will perform serialization without the expected custom behavior.

The reason serialization works like this is because the custom serialization behavior of a class only applies to the fields declared in that class, and cannot be shared with its descendants. Thus, custom serialization methods are private. Descendants of the class are likewise expected to privately implement extra logic as required to serialize and deserialize data for their own declared fields.


## Bad Practice

```java
// readObjectNoData should return void!
    private int readObjectNoData() throws ObjectStreamException {
        // ...
    }

    // readObject should not be public!
    public void readObject(ObjectInputStream in) throws IOException, ClassNotFoundException {
        // ...
    }

    // writeObject should not throw ClassNotFoundException!
    private void writeObject(ObjectOutputStream object) throws ClassNotFoundException {
        // ...
    }
```

## Recommended
Specify the method signatures for these methods as expected by Java.

