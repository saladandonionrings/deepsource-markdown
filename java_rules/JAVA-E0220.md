# `readLine` result is read without a null check
**ID:** `JAVA-E0220` | **Lien:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-E0220)

![Critical](https://img.shields.io/badge/severity-critical-red) ![Bug Risk](https://img.shields.io/badge/type-bug_risk-green)

The result of invoking `readLine()` is read without checking to see if it is null.


## Bad Practice
If there are no more lines of text to read, `readLine` will return null and dereferencing that will generate a null pointer exception.


```java
String a = bufReader.readLine();
int b = a.length; // Will throw an NPE if a is null.
```

## Recommended

```java
String a = bufReader.readLine();

if (a == null) {
    // do something....
}
```
Unless you are absolutely confident that this will not happen (though it is a sensible thing to do even then), check if the result is null and perform an appropriate action if it is.

