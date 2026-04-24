# Non-constant string passed to `execute` or `addBatch` method on an SQL statement
**ID:** `JAVA-S0082` | **Link:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-S0082)

![Critical](https://img.shields.io/badge/severity-critical-red)![Security](https://img.shields.io/badge/type-security-red)

The method invokes the `execute` or `addBatch` method on an SQL statement with a `String` that seems to be dynamically generated. This can allow SQL injection attacks to occur.


## Example

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
This is clearly not a statement that can be safely executed in production, and the attacker may be able to freely access the data retrieved.


## Recommended Action
There are a number of solutions to this issue:

* Use prepared statements, they can perform validation and will escape strings properly* Use an ORM, which will perform the validation for you* Perform filtering and validation for parameters yourself with whitelists or converting to native types. This may allow for edge cases to occur, so only use this as a last resort

```java
String user = request.getParameter("user");
String pass = request.getParameter("pass");

String query = "SELECT * FROM users WHERE user = ? AND pass = ?";

PreparedStatement statement = connection.prepareStatement(query);
statement.setString(1, user); // Will be properly escaped
statement.setString(2, pass);

// Execute and use the returned ResultSet as required.
```

## Exceptions
If you know what you are doing and have taken pains to filter untrusted input before creating the query, you can ignore this issue. Review such cases thoroughly if you haven't already done so.

