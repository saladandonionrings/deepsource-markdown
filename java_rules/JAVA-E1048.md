# Call to unsupported method detected
**ID:** `JAVA-E1048` | **Lien:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-E1048)

![Major](https://img.shields.io/badge/severity-major-orange) ![Bug Risk](https://img.shields.io/badge/type-bug_risk-green)

A method that unconditionally throws an [ `UnsupportedOperationException` ](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/lang/UnsupportedOperationException.html) was called. Avoid calling methods that you know will always throw an exception.

This issue is raised when:


## Bad Practice

```java
class SomeClass {
  final void doSomething() {
    throw new UnsupportedOperationException();
  }

  // ...
}
// ...

SomeClass someObject = new SomeClass();

someObject.doSomething();  // Will always throw!
```

## Recommended
Check if you are calling this method on the correct class. If you called this method by accident, consider changing your code to avoid calling it.

