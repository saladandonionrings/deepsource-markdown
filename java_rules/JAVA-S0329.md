# Prepared statements should not be created within a loop
**ID:** `JAVA-S0329` | **Link:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-S0329)

![Critical](https://img.shields.io/badge/severity-critical-red)![Performance](https://img.shields.io/badge/type-performance-white)

The method calls `Connection.prepareStatement` inside the loop passing the constant arguments.

If the `PreparedStatement` should be executed several times there's no reason to recreate it for each loop iteration.

Creating a prepared statement causes the server side database engine to allocate resources for the purpose of efficient execution. If the same statement is recreated for no reason, it could drive the engine to exhaust allocated memory unnecessarily. Modern implementations tend to cache such statements to prevent this kind of exhaustion from occurring ([Oracle DB](http://docs.oracle.com/cd/B10501_01/java.920/a96654/stmtcach.htm) for example) but this behavior must not be relied on.

Move this call outside the loop.

