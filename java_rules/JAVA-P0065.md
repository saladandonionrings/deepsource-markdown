# Explicit invocation of garbage collection is detrimental apart from some benchmarking use cases
**ID:** `JAVA-P0065` | **Lien:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-P0065)

![Major](https://img.shields.io/badge/severity-major-orange) ![Performance](https://img.shields.io/badge/type-performance-white)

This code explicitly invokes garbage collection via `System.gc()` or `Runtime.gc()` . Except for specific use in benchmarking, this is very dubious.

The JVM may choose to freeze the entire application to perform GC, may completely ignore the invocation (if the `-XX:DisableExplicitGC` flag is set for the VM for example) or defer GC for later. Also, it is impossible to say how the garbage collection will take place since there are many factors which affect GC behavior.

Because its behavior is so variable, it cannot be relied on to reduce memory consumption and can in fact actively kill performance instead.


## Bad Practice

```java
System.gc();

// Or

Runtime.getRuntime().gc();
```

## Recommended
Avoid calling `System.gc()` . Instead, consider profiling your application to find the underlying cause of any memory issues that force you to use it.

Profiling your application can provide useful insights into such issues and can help in understanding areas where and how memory usage could be improved.

