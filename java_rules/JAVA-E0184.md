# Return value of `InputStream.skip` should not be ignored
**ID:** `JAVA-E0184` | **Link:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-E0184)

![Major](https://img.shields.io/badge/severity-major-orange)![Bug Risk](https://img.shields.io/badge/type-bug_risk-green)

This method ignores the return value of `java.io.InputStream.skip`. This method may be used to skip a particular number of bytes as provided to it, and returns the actual number of bytes skipped. If the return value is not checked, the caller will not be able to correctly handle the case where fewer bytes were skipped than the caller requested.

This is a particularly insidious kind of bug, because in many programs, skips from input streams usually do skip the full amount of data requested, causing the program to fail only sporadically.

Note that with buffered streams, such as a stream wrapped with `BufferedInputStream`, `skip` will only skip data in the buffer, and will routinely fail to skip the requested number of bytes.

Because of this, it is recommended to always check the return value of `skip` to make sure such errors do not occur.


## Bad Practice

```java
InputStream is = new FileInputStream("a.out");
is.skip(250);
```

## Recommended
Make sure to check how many characters are skipped before performing any other operations on the stream.


```java
int nSkipped = is.skip(250);
if (nSkipped != 250) {
    // ...
}
```
