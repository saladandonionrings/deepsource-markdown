# `readObject` should not be synchronized
**ID:** `JAVA-W1093` | **Link:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-W1093)

![Major](https://img.shields.io/badge/severity-major-orange)![Anti-pattern](https://img.shields.io/badge/type-anti_pattern-purple)

Marking the `readObject` method as `synchronized` is useless, as this method will never be used with an object that is shared across threads.

Using the `synchronized` modifier is thus not required and may actually be confusing.

`readObject` is implemented to specify custom deserialization logic for the JVM to use. It is never called in a multithreaded context by the JVM itself.

Additionally, this method is never meant to be called explicitly by non-JVM code, which means there is never a need to use it with synchronization.

Thus, it is not recommended to add the `synchronized` modifier to `readObject`.


## Bad Practice

```java
private synchronized void readObject(ObjectInputStream oiStream)
        throws IOException, ClassNotFoundException {
    // ...
}
```

## Recommended
Remove the `synchronized` modifier.


```java
private void readObject(ObjectInputStream oiStream)
        throws IOException, ClassNotFoundException {
    // ...
}
```
