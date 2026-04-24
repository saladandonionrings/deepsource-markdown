# Audit: Prepared query may be susceptible to injection attacks
**ID:** `JAVA-A1041` | **Lien:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-A1041)

![Critical](https://img.shields.io/badge/severity-critical-red) ![Security](https://img.shields.io/badge/type-security-red)

Avoid creating prepared SQL queries with non-constant strings.

Prepared queries allow us to ensure that the value of any parameter cannot change the structure of the query itself through the effect of special characters.

However, SQL injections are possible under either (or both) of the following circumstances:

* The prepared query's initial string is dynamically generated (Using string concatenation for example)
* User data is directly used to build the query string at any point.

This issue will be raised if a prepared SQL query string is created using possibly unsanitized input from a source such as a request parameter.


## Bad Practice

```java
String user = request.getParameter("user");
String pass = request.getParameter("pass");
String predicate = request.getParameter("predicate");

// An attacker who could control the value of predicate here could make arbitrary changes to the query!
String query = "SELECT * FROM users WHERE " + predicate + " AND user = ? AND pass = ?";

PreparedStatement statement = connection.prepareStatement(query);
statement.setString(1, user);
statement.setString(2, pass);
```

## Recommended
Here are a few steps you can take to reduce the risk of attacks:

* Sanitize all input by removing any special characters.
* Implement a whitelist of allowed inputs wherever possible. Certain values, such as table and column names cannot be parameterized and if you need their values to be dynamic, you must perform whitelisting on such queries.
* Certain values, such as table and column names cannot be parameterized and if you need their values to be dynamic, you must perform whitelisting on such queries.
* Keep the prepared query's SQL string fixed, and set all parameters using only the placeholder strings.

* Certain values, such as table and column names cannot be parameterized and if you need their values to be dynamic, you must perform whitelisting on such queries.


```java
String user = request.getParameter("user");
String pass = request.getParameter("pass");
String predColumnName = request.getParameter("pcol");
String predColumnValue = request.getParameter("pval");

if (predColumnName == null || !allowedColumns.contains(predColumnName)) {
    // invalid! Do not execute any of the rest of this code!
} else {

    // We cannot use predColumnName as a placeholder parameter, so we directly format the query after ensuring it does not have a forbidden value.
    String query = String.format("SELECT * FROM users WHERE %s = ? AND user = ? AND pass = ?", predColumnName);

    PreparedStatement statement = connection.prepareStatement(query);
    statement.setString(1, user);
    statement.setString(2, pass);
}
```
