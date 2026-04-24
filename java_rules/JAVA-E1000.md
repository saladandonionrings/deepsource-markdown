# A syntax error was found
**ID:** `JAVA-E1000` | **Link:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-E1000)

![Critical](https://img.shields.io/badge/severity-critical-red)![Bug Risk](https://img.shields.io/badge/type-bug_risk-green)

The Java analyzer found a malformed function call, incorrect syntax or some other similar error that will result in a compilation failure.


## Bad Practice

```java
String.format(); // String.format requires at least one argument.
```

## Recommended
Consider using an IDE to highlight such issues as they occur. It is also a good idea to try compiling your code at least once before pushing to ensure that there are no errors preventing compilation.

