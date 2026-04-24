# Local variable that shadows field is written to but not read from
**ID:** `JAVA-S0353` | **Link:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-S0353)

![Critical](https://img.shields.io/badge/severity-critical-red)![Bug Risk](https://img.shields.io/badge/type-bug_risk-green)

A value is assigned to a local variable, but it is not read or used in any subsequent code.

Often, this indicates an error, because the value computed is never used. There is a field with the same name as the local variable. Did you mean to assign to that variable instead?

Avoid shadowing fields with local variables, as it leads to confusion and reduces maintainability.

