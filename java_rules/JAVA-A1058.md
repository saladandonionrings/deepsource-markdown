# Audit: Amazon SimpleDB queries should not be susceptible to injection attacks
**ID:** `JAVA-A1058` | **Lien:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-A1058)

![Critical](https://img.shields.io/badge/severity-critical-red) ![Security](https://img.shields.io/badge/type-security-red)

Amazon SimpleDB queries should not be constructed using unvalidated external data.


## Bad Practice
Avoid directly performing string concatenation to create SQL queries, as this can lead to injection attacks.


```java
String table = request.getParameter("model");

String query = "SELECT * FROM " + table + " WHERE id = '" + id + "'"; // Susceptible to injection!
SelectResult result = conn.select(new SelectRequest(query));
```

## Recommended
In security, allow-lists are more preferable to deny-lists, due to how specific they can be. If possible, narrow down to the absolute minimum the behaviors that are desired within a query, and use external input only to select the behavior required for the specific purpose.

Make sure to sanitize data from files or requests by first passing it through allow-lists.


```java
if (!allowlist.contains(table)) return;

// ...

String query = String.format("SELECT * from %s where id = '%s'", table, id);
```
