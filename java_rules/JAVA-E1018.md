# Audit: Double checked locking is not safe
**ID:** `JAVA-E1018` | **Link:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-E1018)

![Major](https://img.shields.io/badge/severity-major-orange)![Bug Risk](https://img.shields.io/badge/type-bug_risk-green)

This method may contain an instance of double-checked locking of a non-volatile field. This will not work because the [unpredictability of Java's object allocation mechanics](https://stackoverflow.com/a/4926812/6325886) may result in race conditions with other threads.


## Bad Practice
To explain what could go wrong, consider the following code:


```java
public class DoubleCheckedLocking {
    private static Resource internalInstance;

    public static Resource getInstance() {
        if (internalInstance == null) {
            synchronized (DoubleCheckedLocking.class) {
                if (internalInstance == null)
                    internalInstance = new Resource(); // !!!
            }
        }
        return internalInstance;
    }

    static class Resource {
        // ...
    }
}
```
We can notice the following things here:

The root cause of the problem is how the assignment of a new value to `internalInstance` is handled. The JVM is free to change the order of operations with respect to how objects are created and assigned to references. It is possible that `internalInstance` may be assigned a partially constructed object. That is to say, the JVM may first assign `internalInstance` before the code of the `Resource` class's constructor completes.

If a different thread were to call `getInstance()` after `internalInstance` is assigned but before the object it is assigned is properly constructed, one possible sequence of events may occur:

Thread `A`:

* Within synchronized block:`internalInstance`is non-null. The object referenced by`internalInstance`however is not fully constructed.* We do not exit the synchronized block within thread`A`.
Thread `B`:

If thread `B` preempts `A` before `A` has a chance to fully initialize `internalInstance`, a data race would effectively occur with the result being that any further operations done on `internalInstance` in thread `B` will likely throw an exception.


## Recommended
It is not enough to declare `internalInstance` as being static. It is possible and probable, that changes to `internalInstance` within one thread may not be reflected in others. There are a number of ways to remedy this.

**Make the getter method `synchronized`**


```java
public class AlwaysSynchronize {
    private static Resource internalInstance;

    public static synchronized Resource getInstance() {
        if (internalInstance == null) {
            internalInstance = new Resource();
        }
        return internalInstance;
    }

    static class Resource {
        // ...
    }
}
```
While this may increase overhead by some degree, it will get the job done painlessly.

**Use the `volatile` keyword**


```java
class DoubleCheckedVolatileResource {
    private static volatile Resource internalInstance;

    public static Resource getInstance() {
        // We first copy the volatile reference to a local variable to avoid touching the volatile field multiple times when it is already initialized.
        Resource localResource = internalInstance;
        if (localResource == null) {
            synchronized (DoubleCheckedVolatileResource.class) {
                // We do need to access the volatile field again to check for null.
                localResource = internalInstance;
                if (localResource == null) {
                    // We still assign to the local variable because that's the value we will return.
                    internalInstance = localResource = new Resource();
                }
            }
        }
        // Returning the local variable instead of the field will be much faster in the general case where the field has already been initialized.
        return localResource;
    }

    static class Resource {
        // ...
    }
}
```
Here, a local variable holds the value of `internalInstance` to ensure that we do not access the volatile field reference multiple times. Accessing the volatile field can cause issues like partially initialized data becoming visible to other threads. Moreover, volatile field accesses are costlier than that of regular fields, meaning it is always better to reduce the number of times one interacts with such fields.

**Use a static inner holder class**


```java
public class InnerStaticResourceHolder {
    private static class ResourceHolder {
        public static Resource internalInstance = new Resource(); // This will be lazily initialized
    }

    public static Resource getResource() {
        return InnerStaticResourceHolder.ResourceHolder.internalInstance;
    }

    static class Resource {
        // ...
    }
}
```
The static inner `ResourceHolder` class ensures that the `internalInstance` variable will only be initialized once, at the time of its first access. This works because of how static fields of static inner classes are initialized, and is known as the [initialization-on-demand holder pattern](https://en.wikipedia.org/wiki/Initialization-on-demand_holder_idiom). However, this method cannot be used to create instance-specific resources, only static ones. That is, we cannot have a version of `InnerStaticResourceHolder` which will allow multiple instances to have their own unique lazy initialized versions of `internalInstance`.

It is also inadvisable to use this pattern when lazy initialization of the variable could fail. If the initialization of the static variable fails, the first call to `getResource()` would result in an `ExceptionInInitializerError` and subsequent calls would result in `NoClassDefFoundError`s.

