# `System.exit()` should only be invoked within application entry points
**ID:** `JAVA-W0060` | **Link:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-W0060)

![Major](https://img.shields.io/badge/severity-major-orange)![Bug Risk](https://img.shields.io/badge/type-bug_risk-green)

This method invokes `System.exit()`, and is called by other code. This can prevent proper error handling and debugging.

Invoking `System.exit()` shuts down the entire Java virtual machine. This should only been done when it is appropriate. Such calls make it hard or impossible for your code to be invoked by other code, since an error that causes `System.exit()` to be invoked cannot be handled by the calling code at all.


## Bad Practice

```java
if (input == null) System.exit(1);
```

## Recommended
Consider throwing an exception on failure instead.


```java
if (input == null) throw new InvalidInputException();
```

## Exceptions
If the code is intended to be called only by an application entrypoint, this issue can safely be ignored. Ensure that such cases are well documented.

