# Closeable values should not be injected via `@Provides` annotated methods
**ID:** `JAVA-E1103` | **Link:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-E1103)

![Major](https://img.shields.io/badge/severity-major-orange)![Bug Risk](https://img.shields.io/badge/type-bug_risk-green)

Avoid marking methods that return [`Closeable`](https://docs.oracle.com/javase/8/docs/api/java/io/Closeable.html) with any dependency injection annotations such as `@Provides` or `@Inject`, as this could cause resource management problems.

This issue respects suppression via `@SuppressWarnings("CloseableProvides")`.


## Bad Practice
Consider this method that "provides" a `FileOutputStream` value through DI:


```java
@Provides
FileOutputStream provideFileStream() {
    return new FileOutputStream(someFile);
}
```
Note that this method would return a new output stream every time it is called.

Now, what if we have some component in our application that requires a file output stream to be injected into it?


```java
class DependencyComponent {
  private final FileOutputStream fos;
  
  @Inject
  DependencyComponent(FileOutputStream fos) { // fos would be provided through provideFileStream()!
     this.fos = fos; 
  }
  
  // ...
}
```
If multiple instances of `DependencyComponent` were to exist, `provideFileStream()` would be called that many times as well, meaning there would be multiple different file streams pointing to the same file. It is possible that the application could run out of file descriptors if there are too many streams opened.

If the file stream is supposed to be a singleton (if `provideFileStream()` were marked with `@Singleton`), this would mean the same stream is shared across multiple instances of `DependencyComponent`. This would be a different kind of problem; which instance of `DependencyComponent` would be responsible for closing the singleton?

For a more in-depth exploration of the problem, see [this page](https://errorprone.info/bugpattern/CloseableProvides) from ErrorProne.


## Recommended
Instead of injecting a resource directly, provide a wrapper that creates and destroys the resource only when needed.


```java
class FileInterface {
    File file;
    
    public FileInterface(File file) {
        this.file = file;
    }

    // A file stream is created and destroyed entirely within this method alone.
    public void doWithFile(Consumer<FileOutputStream> action) {
        try (FileOutputStream stream = new FileOutputStream(this.file)) {
            action.accept(stream);
        }
    }
}

// ...

@Provides
FileInterface provideFileInterface() {
    return new FileInterface(someFile);
}
```
The `FileInterface` type now has a method `doWithFile` which automatically opens a file output stream, performs some action on it, and closes the stream once done. This way, the resource is only created when required.

