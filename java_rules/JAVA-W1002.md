# Exceptions must not be thrown in finalizers
**ID:** `JAVA-W1002` | **Lien:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-W1002)

![Major](https://img.shields.io/badge/severity-major-orange) ![Anti-pattern](https://img.shields.io/badge/type-anti_pattern-purple)

Finalizers ( `finally` blocks) are used to perform cleanup after a try-catch block has executed, regardless of whether an exception was previously thrown. Throwing an exception within a finalizer will essentially discard any previously thrown exception, meaning all existing context on the original exception will be lost from that point onwards. This will make bugs difficult to detect, and must be avoided.


## Bad Practice

```java
boolean notFound = false;
try {
    if (someCond) notFound = true;

    someMethodThatMayThrowNPE(); // NPE thrown...
} catch (FileNotFoundException e) {
    ...
} finally {
    // An exception is thrown here even if there is already another exception thrown in the try block.
    if (notFound) throw new FileNotFoundException();
}
```
If an exception was thrown but not caught in the `try` block, and a `FileNotFoundException` is thrown in the finalizer, only the last exception thrown (The one from the finalizer) will ultimately be bubbled up the rest of the callstack. Important context which could be used to debug this issue is lost when this occurs.


## Recommended
Never throw exceptions within finalizers.

