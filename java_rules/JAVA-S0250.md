# Attempt to close a null value detected
**ID:** `JAVA-S0250` | **Link:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-S0250)

![Critical](https://img.shields.io/badge/severity-critical-red)![Bug Risk](https://img.shields.io/badge/type-bug_risk-green)

`close()` is being invoked on a value that is always null. If this statement is executed, a null pointer exception will occur.

Another serious issue is the fact that the resource that is meant to be closed is not closed.

