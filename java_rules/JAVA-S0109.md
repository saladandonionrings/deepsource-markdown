# Method does not check if an argument is null
**ID:** `JAVA-S0109` | **Link:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-S0109)

![Critical](https://img.shields.io/badge/severity-critical-red)![Bug Risk](https://img.shields.io/badge/type-bug_risk-green)

A parameter to this method has been identified as a value that should always be checked to see whether or not it is null, but it is being dereferenced without a preceding null check.

This can very easily lead to null pointer exceptions in your code, and must be avoided without exception.

Always perform null checks on values that could be null when they are passed to methods.

