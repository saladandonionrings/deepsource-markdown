# JUnit test class overrides `tearDown` but does not invoke `super.tearDown()`
**ID:** `JAVA-S0338` | **Link:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-S0338)

![Major](https://img.shields.io/badge/severity-major-orange)![Bug Risk](https://img.shields.io/badge/type-bug_risk-green)

This class inherits from JUnit's `TestCase` class and implements the `tearDown` method. The `tearDown` method should call `super.tearDown()`, but doesn't.

