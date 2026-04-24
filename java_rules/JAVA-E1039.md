# Annotation check will always return false
**ID:** `JAVA-E1039` | **Lien:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-E1039)

![Major](https://img.shields.io/badge/severity-major-orange) ![Bug Risk](https://img.shields.io/badge/type-bug_risk-green)

Using [ `AnnotatedElement.isAnnotationPresent()` ](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/lang/reflect/AnnotatedElement.html#isAnnotationPresent(java.lang.Class)) to check for an annotation whose retention policy is anything but [ `RetentionPolicy.RUNTIME` ](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/lang/annotation/RetentionPolicy.html#RUNTIME) will always evaluate to `false` .

If you can, change the retention policy of the offending annotation to runtime. Otherwise, consider using a different approach to check for such information.

If an annotation's retention policy is not explicitly set to be `RetentionPolicy.RUNTIME` , it will not be visible at runtime through reflection.


## Bad Practice

```java
@Retention(RetentionPolicy.SOURCE)
@interface Bad {
    // ...
}

// The default retention policy is RetentionPolicy.CLASS.
@interface AlsoBad {}

// ... elsewhere ...

class SomeClass {
    @Bad
    public void someMethod() {
        // ...
    }
}

// ... elsewhere ...

Method someMethod = SomeClass.class.getMethod("someMethod");

// This check will always fail!
if (someMethod.isAnnotationPresent("Bad") || someMethod.isAnnotationPresent("AlsoBad")) {
    // ...
}
```

## Recommended
If possible, set the retention policy of the offending annotation to `RetentionPolicy.RUNTIME` . Otherwise, consider using a different method to check for the condition represented by the annotation.


```java
@Retention(RetentionPolicy.RUNTIME)
@interface Good {}
```
