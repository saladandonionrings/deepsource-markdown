# Test files should contain tests
**ID:** `JAVA-W1088` | **Lien:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-W1088)

![Minor](https://img.shields.io/badge/severity-minor-yellow) ![Anti-pattern](https://img.shields.io/badge/type-anti_pattern-purple)

Classes that look like test cases should contain tests.

This issue is reported when a file within a test directory looks like a test file (has "Test" in its name, or contains classes with test framework related annotations) but contains no actual test code.

This issue will be reported if no symbols related to any of the following frameworks are found:

* Junit 3
* Junit 4
* Junit 5
* TestNG
* ArchUnit


## Bad Practice
Avoid declaring classes that seem like tests but don't contain any test cases.


```java
class SomethingTest {
    // no test methods.
}
```

## Recommended
Give the class a better name, or remove it altogether if there is no need for it.

