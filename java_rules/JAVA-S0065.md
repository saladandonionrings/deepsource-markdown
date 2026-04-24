# Explicit invocation of garbage collection is detrimental apart from some benchmarking use cases
**ID:** `JAVA-S0065` | **Lien:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-S0065)

![Major](https://img.shields.io/badge/severity-major-orange) ![Performance](https://img.shields.io/badge/type-performance-white)

This code explicitly invokes garbage collection via `System.gc` or `Runtime.gc` . Except for specific use in benchmarking, this is very dubious.

The JVM may choose to freeze the entire application to perform GC, may completely ignore the invocation (if the `-XX:DisableExplicitGC` flag is set for the VM for example) or defer GC for later. Also, it is impossible to say how the garbage collection will take place since there are many factors which affect GC behavior.

Because its behavior is so variable, it cannot be relied on to reduce memory consumption and can in fact actively kill performance instead.


## Recommended
Do not use `System.gc` in your code. Instead, consider redesigning your code so that it uses less memory in the long run. If you feel the need to call `System.gc` , it may be that your application uses more memory than what you believe is necessary. This may be due to bad object allocation and usage patterns, which prevent the garbage collector from considering many objects for garbage collection.

Profiling your application can provide useful insights into such issues and can help in understanding areas where and how memory usage could be improved.

