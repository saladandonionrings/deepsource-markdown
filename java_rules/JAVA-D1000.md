# Undocumented class found
**ID:** `JAVA-D1000` | **Lien:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-D1000)

![Minor](https://img.shields.io/badge/severity-minor-yellow) ![Documentation](https://img.shields.io/badge/type-documentation-lightgrey)

This class does not have any documentation.

Consider adding a documentation comment to explain its use.

While it may seem like the functionality of a class is perfectly obvious, any consumers of your API may not be able to pick up on certain details.


## Bad Practice
Consider a case where the class given below can be instantiated and provides certain functionalities within each instance in a thread-safe manner, perhaps it is a rest API client.

If there is no documentation comment on the class, it is not immediately obvious that the class is thread safe. Thus, multiple instances of the class may be created to perform operations concurrently, using up both memory as well as OS resources like sockets. If it were known from the beginning that the class were thread safe, the user would not need to create unnecessary extra instances of `SomeClass` .


```java
class SomeClass {
    // ...
}
```

## Recommended
Make sure to add useful information regarding the usage or implementation of a particular declaration, so that anything about it which can't be understood from the name or some other cue is correctly conveyed.


```java
/**
 * Instances of this class are used to perform xyz action.
 *
 * This class is thread safe and the same instance can be used over multiple threads.
 */
class SomeClass {
    // ...
}
```

## Exceptions
This issue will not be reported for model entity classes. If there is any non-obvious behavior associated with a particular class however, do consider documenting it.

