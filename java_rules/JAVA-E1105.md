# Incorrect main method signature detected
**ID:** `JAVA-E1105` | **Lien:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-E1105)

![Critical](https://img.shields.io/badge/severity-critical-red) ![Bug Risk](https://img.shields.io/badge/type-bug_risk-green)

The main method requires a very specific signature for Java to recognise it as such. Ensure that the main method is public, static, returns void and takes a single `String` array as an argument.

This checker respects suppression by marking the method or type with `@SuppressWarnings("IncorrectMainMethod")`


## Bad Practice
The `main` method below is public and returns `void` but is not static. This method will not be recognized by Java and if you try to run it, the JVM will exit with an error.


```java
class Example {
    public void main(String[] args) {
        // ...
    }
}
```

## Recommended
Use the correct signature.


```java
public static void main(String[] args) {
    // ...
}
```
If this method is not intended to be the actual "main" method, consider renaming it to something like "innerMain" or "realMain" to signify that the actual "main" method is something entirely different.

