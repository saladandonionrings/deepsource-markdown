# Stream may be left unclosed
**ID:** `JAVA-S0269` | **Lien:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-S0269)

![Critical](https://img.shields.io/badge/severity-critical-red) ![Anti-pattern](https://img.shields.io/badge/type-anti_pattern-purple)

The method creates an IO stream object but does not appear to do any of the following:

* Assign the stream to any fields
* Pass it to other methods that might close it
* Return it
* Close the stream on all possible exception paths out of the method


```java
void method() {

    InputStream stream = new FileInputStream(...); // Open a stream...

    // ...
    
    // We don't close, return or pass the stream to another function it before this function ends.
    return;
}
```
It is in general a good practice to close a stream object when you are done with it. While the stream could possibly be closed when it is garbage collected, this is not guaranteed, and relying on the garbage collector to dispose of used resources is heavily discouraged.

Ensure that the stream is closed, preferably through constructs such as try-with-resources or `finally` blocks. If no exceptions can occur, ensure that the stream is closed at the end of the method.

Note that some stream types ( `ByteArrayInputStream` for example) do not need to be closed since they don't actually access any closable resources. It is still a good practice, however, to call their close method appropriately.

See [this discussion](https://stackoverflow.com/questions/1522370/does-input-outputstreams-close-on-destruction) for more perspective on this issue.

