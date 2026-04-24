# Prepared statements should not be created within a loop
**ID:** `JAVA-P1003` | **Lien:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-P1003)

![Critical](https://img.shields.io/badge/severity-critical-red) ![Performance](https://img.shields.io/badge/type-performance-white)

This method calls `Connection.prepareStatement()` inside a loop with constant arguments. This is inefficient; move this call outside the loop.

Prepared statements can be costly to create (depending on the server implementation) because both client side and server side actions may be required to successfully create a prepared statement. For example, the server may cache the query, or generate an execution plan for the query before-hand. If a prepared statement is repeatedly created, there is a risk of various side effects occurring, such as memory or resource exhaustion, and unnecessary CPU utilization. Though modern implementations tend to cache such statements to prevent this kind of exhaustion from occurring ( [Oracle DB](http://docs.oracle.com/cd/B10501_01/java.920/a96654/stmtcach.htm) for example), this behavior must not be relied on.


## Bad Practice

```java
for (...) {
    PreparedStatement statement = connection.prepareStatement("SELECT * FROM users WHERE user = ?;");

    // Use the statement.
}
```

## Recommended
Move this call outside the loop.


```java
PreparedStatement statement = connection.prepareStatement("SELECT * FROM users WHERE user = ?;");

for (...) {
    // Use the statement.
}
```
