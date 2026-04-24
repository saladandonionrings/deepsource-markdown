# Audit: SQL query may be susceptible to injection attacks
**ID:** `JAVA-A1042` | **Link:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-A1042)

![Critical](https://img.shields.io/badge/severity-critical-red)![Security](https://img.shields.io/badge/type-security-red)

It is not a good idea to use a dynamically generated string (such as a string created with concatenation, or a request parameter) to execute an sql query.

This issue will be raised when code that is commonly vulnerable to injection attacks, such as request processing code appears to be using possibly unsanitized data to create an SQL query through methods such as [`Statement.addBatch()`](https://docs.oracle.com/javase/7/docs/api/java/sql/Statement.html#addBatch(java.lang.String)) or [`Statement.execute()`](https://docs.oracle.com/javase/7/docs/api/java/sql/Statement.html#execute(java.lang.String)).


## Bad Practice

```java
String user = request.getParameter("user");
String pass = request.getParameter("pass");

String query = "SELECT * FROM users WHERE user = '" + user + "' AND pass = '" + pass + "'"; // Unsafe
```
In the example above, `user` and `pass` are untrusted values which have not been sanitized before use. Consider a case where `user` has the value `"' OR 1=1 --"`. The query string then becomes:


```java
SELECT * FROM users WHERE user = '' OR 1=1 -- AND pass = '...'
```
Here, `--` is the SQL comment token and turns the rest of the line after it into a comment. This line is now equivalent to:


```java
SELECT * FROM users WHERE 1=1
```
Since `1=1` will always evaluate to a true value, it will not be necessary to check for the value of `user`, leading to the final form of the statement:


```java
SELECT * FROM users
```
This is clearly not a statement that can be safely executed in production, and would likely become an important step in an attacker's chain of exploitation.


## Recommended
There are a number of solutions to this issue:

* Use prepared statements, they can perform validation and will escape strings properly.* Use an ORM, which will perform the validation for you.* Perform filtering and validation for parameters yourself with whitelists or converting to native types. This may allow for edge cases to occur, so only use this as a last resort.
Here is an example of using a prepared statement to write the same query:


```java
String user = request.getParameter("user");
String pass = request.getParameter("pass");

String query = "SELECT * FROM users WHERE user = ? AND pass = ?";

PreparedStatement statement = connection.prepareStatement(query);
statement.setString(1, user); // Will be properly escaped
statement.setString(2, pass);

// Execute and use the returned ResultSet as required.
```
