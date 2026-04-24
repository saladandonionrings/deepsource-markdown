# Return value of `InputStream.read()` should not be ignored
**ID:** `JAVA-E0183` | **Lien:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-E0183)

![Major](https://img.shields.io/badge/severity-major-orange) ![Bug Risk](https://img.shields.io/badge/type-bug_risk-green)

This method ignores the return value of one of the variants of `java.io.InputStream.read` . All variants of `read` return the number of bytes read. If the return value is not checked, the caller may ignore some bytes of input (for the 0 argument overload) or may be unable to handle cases where the expected amount of data was not received by the caller.

This is a particularly insidious kind of bug, because in many programs, reads from input streams usually do read the full amount of data requested, causing the program to fail only sporadically.


## Bad Practice

```java
Byte[] buffer = new Byte[1024];

InputStream is = new FileInputStream("a.out");
is.read(buffer); // Return value ignored!
```

## Recommended
Checking the returned value is recommended.


```java
int nBytes = is.read(buffer);

if (nBytes < expectedSize) {
    // ...
}
```
