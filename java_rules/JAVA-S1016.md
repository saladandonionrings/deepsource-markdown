# Prepared statements must not be generated from dynamically created strings
**ID:** `JAVA-S1016` | **Lien:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-S1016)

![Critical](https://img.shields.io/badge/severity-critical-red) ![Security](https://img.shields.io/badge/type-security-red)

The code creates an SQL prepared statement from a `String` that was formed dynamically. This may be vulnerable to SQL injection attacks.


## Bad Practice
While prepared statements are generally safer than directly building queries and executing them, their effectiveness is reduced when they are created dynamically, especially when the queries are created from untrusted input. One use case of this is when parameters such as table or column names need to be selected dynamically. Prepared statements do not generally allow such values to be parameterized, so we may end up resorting to string concatenation again:


```java
// ...

String table = request.getParameter("table");
String user = request.getParameter("user");
String pass = request.getParameter("pass");

String query = "SELECT * FROM " + table + " WHERE user = ? AND pass = ?"; // Unsafe...

PreparedStatement pStmt = connection.prepareStatement(query);

pStmt.setString(1, user);
pStmt.setString(2, pass);

ResultSet res = pStmt.execute();

// ...
```
A savvy attacker would be free to pass an input such as `"users WHERE (user = ? AND pass = ?) OR 1=1 --"` in the table parameter which would allow them to trick the backend into fetching all rows of the `users` table. The resultant query can be interpreted as shown below:


```java
SELECT * FROM users WHERE (user = ? AND pass = ?) OR 1=1 -- WHERE user = ? AND pass = ?
|   /* Commented part is ignored; (a AND b) OR true always evaluates to true. */
V
SELECT * FROM users WHERE 1=1
|   /* We don't need the where clause either. */
V
SELECT * FROM users
```

## Recommended
There are a number of strategies that could prevent this:

* The `table` parameter could be checked against a whitelist to ensure only valid table names are accepted


```java
String table = request.getParameter("table");

if (!whitelist.contains(table)) {
    // ...
} else {
    // ...
}
```
* Escape any special characters in user input. Note that if this is not done properly, you may end up with a situation such as [this](https://blog.jdriven.com/2017/10/sql-injection-prepared-statement-not-enough/) ...

Here's an example usage of ESAPI with Oracle SQL (Example taken from OWASP's SQL Injection Cheat Sheet):


```java
Codec ORACLE_CODEC = new OracleCodec();
String query = "SELECT user_id FROM user_data WHERE user_name = '"
    + ESAPI.encoder().encodeForSQL( ORACLE_CODEC, req.getParameter("userID"))
    + "' and user_password = '"
    + ESAPI.encoder().encodeForSQL( ORACLE_CODEC, req.getParameter("pwd")) +"'";
```
* Predefine allowed values as constants, and make the client send an identifier (an integer for example) that is mapped to one of the allowed values.


```java
int querySelector = Integer.parseInt(request.getParameter("query"));

if (querySelector < 0 || querySelector >= QUERIES.size) {
    // ...
}

conn.execute(QUERIES[querySelector]);
```

## Exceptions
If there is a solid guarantee that such an attack cannot occur, this issue can be safely ignored.

