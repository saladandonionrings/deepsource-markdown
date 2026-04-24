# `System.runFinalizersOnExit`/`Runtime.runFinalizersOnExit` are unsafe and must not be used
**ID:** `JAVA-E0061` | **Lien:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-E0061)

![Critical](https://img.shields.io/badge/severity-critical-red) ![Bug Risk](https://img.shields.io/badge/type-bug_risk-green)

Never call `System.runFinalizersOnExit` or `Runtime.runFinalizersOnExit` for any reason: they are among the most dangerous methods in the Java libraries. -- Joshua Bloch

In addition to the dangers alluded to by the quote above, this function has been removed from Java's standard library since version 11, and its usages should be removed in the interest of forward compatibility with later Java versions.

These methods have long been deprecated due to the fact that they are inherently unsafe. `runFinalizersOnExit` causes the `finalize` method of any live object which has defined one to execute before the JVM shuts down. This may cause issues like resources and other objects being disposed of while in use, leading to an inconsistent application state, or even file system corruption.


## Bad Practice

```java
BufferedReader br = new BufferedReader(new FileReader(path))

// br is not closed after usage ...

// ...

System.runFilnalizersOnExit(); // Not recommended
```

## Recommended
The recommended way to handle disposal of such objects is to use language constructs such as [try-with-resources](https://docs.oracle.com/javase/tutorial/essential/exceptions/tryResourceClose.html) or by manually closing resources when they are no longer needed.


```java
try (BufferedReader br = new BufferedReader(new FileReader(path))) {
    // ...
} catch (Exception e) {
    // ...
}
// br automatically closes itself after the try block
```
Ensuring that the application is not built in a way that depends on the garbage collection behavior of the JVM is also recommended, since Java GC is not deterministic and is not always guaranteed to function the same way.

