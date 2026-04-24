# Inefficient `OutputStream` implementation
**ID:** `JAVA-P1002` | **Lien:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-P1002)

![Major](https://img.shields.io/badge/severity-major-orange) ![Performance](https://img.shields.io/badge/type-performance-white)

This class appears to inherit the `write(byte[], int, int)` method from `java.io.OutputStream` or `java.io.FilterOutputStream` , which renders it an inefficient implementation.

Always override `write(byte[], int, int)` suitably when inheriting from these types to avoid using an inefficient implementation.

The default implementation of the array based `write` methods in `OutputStream` and `FilterOutputStream` just call the `write(int)` method in a loop.

Taken from `java.io.OutputStream` 's source code:


```java
public void write(byte b[], int off, int len) throws IOException {
    Objects.checkFromIndexSize(off, len, b.length);
    // len == 0 condition implicitly handled by loop bounds
    for (int i = 0 ; i < len ; i++) {
        write(b[off + i]); // A straightforward loop...
    }
}
```
This is sub-optimal and will reduce write performance. Unless a custom `OutputStream` delegates the array `write` method to a more efficient implementation, or implements the method itself, it is likely that writing multiple bytes at once will be handled inefficiently.


## Bad Practice

```java
public class CustomOutputStream extends OutputStream {
    FileOutputStream underlyingImpl;

    @Override
    public void write(int b) {
        underlyingImpl.write(b);
    }

    // This class delegates the implementation of the array write method to its parent class...
}
```

## Recommended
Make sure to override the array `write` method and either implement it efficiently, or pass on the data to a good implementation.


```java
public class CustomOutputStream extends OutputStream {
    FileOutputStream underlyingImpl;

    @Override
    public void write(int b) {
        underlyingImpl.write(b);
    }

    @Override
    public void write(byte[] b, int off, int len) {
        // FileOutputStream efficiently implements this method.
        underlyingImpl.write(b, off, len);
    }

}
```
