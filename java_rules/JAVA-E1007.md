# Jump statements must not be used within `finally` blocks
**ID:** `JAVA-E1007` | **Link:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-E1007)

![Major](https://img.shields.io/badge/severity-major-orange)![Bug Risk](https://img.shields.io/badge/type-bug_risk-green)

If jump statements such as `break`, `continue`, `return` or `throw` are used within a `finally` block, any exception thrown in the `try` or `catch` blocks encountered before will be ignored, and information regarding the underlying error which caused the exception will be lost.

Avoid using such statements to jump outside a `finally` block.

`finally` blocks will run regardless of whether an exception occurred or not. Because of this, any exception thrown within a `try` or `catch` block associated with a `finally` block will not be propagated until the `finally` block has executed. If any control flow statements are used to exit out of the `finally` block before completion, the previously thrown exception will be ignored and execution will continue as if there was no exception.


## Bad Practice

```java
outer: while (...) {
    try {
        // ...
    } catch (...) {
        // ...
    } finally {
        // ...

        // Using return will discard the exception and return immediately,
        // effectively returning successfully.
        if (condition) return;


        // Even break and continue will ignore any exception when encountered within a finally block.
        if (something) break;
        else if (somethingElse) continue;

        for (int i = 0; i < MAX; i++) {
            // ...

            if (thing) break outer; // Don't jump to labels outside the finally block!
        }


        // Throwing an exception in a finally block will cause any
        // previous exception to be ignored and forgotten.
        throw new RuntimeException(...);
    }
}
```

## Recommended
Avoid using control flow statements to jump out of `finally` blocks.


## Exceptions
It is okay to use jump statements when they keep control flow within the `finally` block:


```java
try { ... } catch (...) { ... } finally {

    inner: while(...) {
        for (...) {

            if (thing) break; // this is okay.

            else if (otherThing) break inner; // this is also okay.
        }
    }

}
```
