# @VisibleForTesting/@TestOnly annotated methods/constructors should not be used in non-test code
**ID:** `JAVA-A1067` | **Link:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-A1067)

![Major](https://img.shields.io/badge/severity-major-orange)![Anti-pattern](https://img.shields.io/badge/type-anti_pattern-purple)

A method/constructor that is marked with annotations such as `@VisibleForTesting` or `@TestOnly` should not be called from non-test code, as such declarations are only meant to be used as test helpers.

Remove/replace the usage with an alternative that does not use such methods if possible.

If this issue does not apply in your case, mark the line where it is reported with a `// skipcq: JAVA-A1067` comment.


## Bad Practice

```java
class SomeClass
    @TestOnly
    void onlyForTests() {
        // ...
    }
}

class SomeOtherClass {   
    void nonTestMethod(SomeClass someObject) {
        someObject.onlyForTests(); // Bad!
    }
}
```

## Recommended
Avoid using such methods unless there is absolutely no other way, and ensure that the usage of such methods cannot trigger any bugs.

