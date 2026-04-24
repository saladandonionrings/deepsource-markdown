# Possible database resource leak detected
**ID:** `JAVA-S0327` | **Link:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-S0327)

![Critical](https://img.shields.io/badge/severity-critical-red)![Anti-pattern](https://img.shields.io/badge/type-anti_pattern-purple)

The method creates a database resource (such as a database connection or row set), does not assign it to any fields, pass it to other methods, or return it, and does not appear to close the object on all exception paths out of the method.

Failure to close database resources on all paths out of a method may result in poor performance, and could cause the application to have problems communicating with the database.

It is recommended to explicitly close the indicated resource, or keep track of it until the point when it must be closed.

