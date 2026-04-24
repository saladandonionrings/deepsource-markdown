# JUnit test class overrides `setUp` but does not invoke `super.setUp()`
**ID:** `JAVA-S0337` | **Link:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-S0337)

![Major](https://img.shields.io/badge/severity-major-orange)![Bug Risk](https://img.shields.io/badge/type-bug_risk-green)

This class inherits from JUnit's `TestCase` class and implements the `setUp()` method. The `setUp` method should call `super.setUp()`, but doesn't.

