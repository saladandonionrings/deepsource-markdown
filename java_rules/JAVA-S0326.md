# Database resource may not be closed on return
**ID:** `JAVA-S0326` | **Lien:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-S0326)

![Critical](https://img.shields.io/badge/severity-critical-red) ![Bug Risk](https://img.shields.io/badge/type-bug_risk-green)

The method creates a database resource (such as a database connection or row) but does not appear to do any of the following:

* Assign the resource to any fields
* Pass it to other methods that might close it
* Return it
* Close the resource object on all possible exception paths out of the method

Failure to close database resources on all paths out of a method may result in poor performance, and could cause the application to have problems communicating with the database. Ensure that the resource is closed no matter how the method is exited.

