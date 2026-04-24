# Arguments to String.format must match the provided format string
**ID:** `JAVA-E1001` | **Link:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-E1001)

![Major](https://img.shields.io/badge/severity-major-orange)![Bug Risk](https://img.shields.io/badge/type-bug_risk-green)

`String.format` accepts arguments based on the content of the format string provided to it. If the format string's specifiers do not match the rest of the arguments provided, `String.format` will raise an exception at runtime.


## Bad Practice
Using the wrong number of parameters as specified by the format string will result in an `IllegalFormatException` being throwm.


```java
String.format("%d", 1, 2); // Extra parameters.

String.format("%d, %d, %d", 1); // Not enough parameters.
```

## Recommended
The number and types of format specifiers in the format string must match the provided arguments.


```java
String.format("%d", 1);
```

## Exceptions
This issue will not be thrown when arguments are referred according to index, as it could be that the same argument may be used more than once.

