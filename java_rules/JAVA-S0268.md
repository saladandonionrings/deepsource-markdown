# Stream does not appear to be closed after use
**ID:** `JAVA-S0268` | **Link:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-S0268)

![Critical](https://img.shields.io/badge/severity-critical-red)![Anti-pattern](https://img.shields.io/badge/type-anti_pattern-purple)

The method creates an IO stream object, does not assign it to any fields, pass it to other methods that might close it, or return it, and does not appear to close the stream on all paths out of the method.

While the stream could possibly be closed when it is garbage collected, it is not guaranteed, and relying on the garbage collector to dispose of used resources is an exceptionally bad habit.

Ensure that the stream is closed, preferably through constructs such as try-with-resources or `finally` blocks, or manually call `close()` on the resource at the end of the method.

See [this discussion](https://stackoverflow.com/questions/1522370/does-input-outputstreams-close-on-destruction) for more perspective on the behavior of java streams.

