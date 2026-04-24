# Mutable fields should not directly be returned
**ID:** `JAVA-S1049` | **Link:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-S1049)

![Major](https://img.shields.io/badge/severity-major-orange)![Security](https://img.shields.io/badge/type-security-red)

A mutable field (which is either an array type or a class with public non-final fields) is returned directly (without being copied). This could result in the internal state of your API being exposed, or worse, open to manipulation.

The side effects of modifying the returned data can range from unexplainable crashes to data theft or injection attacks.


## Bad Practice

```java
class FilteredOperationPerformer {
    private final String[] blacklist = new String[] { "a", "b", "c" };

    String[] getBlacklist() {
        return blacklist; // Bad!
    }

    // ...
}

// Elsewhere...

FilteredOperationPerformer fop = new FilteredOperationPerformer();

String[] blacklistExternal = fop.getBlacklist();

blacklistExternal[0] = "Something else";

System.out.println("External blacklist entries are:");
for (String s : blacklistExternal) {
    System.out.println(s);
}

System.out.println("
Internal blacklist entries are:");
for (String s : fop.getBlacklist()) {
    System.out.println(s);
}
```
The output of this code looks like this:


```java
External blacklist entries are:
Something else
b
c

Internal blacklist entries are:
Something else                  <---- !!!
b
c
```
While the expectation may be that the internal `blacklist` value would not be affected by a change in an external value, it is still affected. This is because the reference returned by `getBlacklist()` points to the same `String[]` object as the inner `blacklist` value.


## Recommended
Use the `Arrays.copyOf()` method to make a copy of an array when returning it.


```java
String[] getBlacklist() {
    return Arrays.copyOf(blacklist);
}
```
If you need to expose a field of a mutable type, either create a new object and copy over the data and return the copied object, or consider adding a copy method to the mutable type which would create a new copy.


```java
SomeMutableClass getInternalData() {

    // Here, a copy constructor has been defined
    // for the `SomeMutableClass` type which will
    // properly copy over all fields of the object.
    return new SomeMutableClass(internalData);
}
```
Deep copies in Java are not very well supported and generally rely on tricks like [abuse of the serialization API or using external libraries](https://stackoverflow.com/questions/64036/how-do-you-make-a-deep-copy-of-an-object) to clone objects. Whatever method you choose, be aware of the risks and pitfalls of performing a clone operation on an object.

