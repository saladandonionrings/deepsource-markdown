# Audit required: Risk of possible SQL injection vector through string-based query construction
**ID:** `BAN-B608` | **Lien:** [DeepSource](https://deepsource.com/directory/python/issues/BAN-B608)

![Major](https://img.shields.io/badge/severity-major-orange) ![Security](https://img.shields.io/badge/type-security-red)

Constructing SQL query using user provided data is insecure. It makes application vulnerable to [SQL injection]( [SQL injection](https://owasp.org/www-community/attacks/SQL_Injection) ) attacks.

An SQL injection attack consists of the insertion or “injection” of an SQL query via the input data given to an application. It is a very common attack vector. Unless care is taken to sanitize and control the input data when building such SQL statement strings, an injection attack becomes possible. It is possible for an attacker to craft queries to read, modify, or delete sensitive information from the database and sometimes even shut it down or execute arbitrary operating system commands.

It is recommended to ensure that the user-provided data is properly escaped and validated. Modern database adapters come with built-in tools for preventing Python SQL injection by using query parameters.

Since this is an audit issue, some occurrences may be harmless here. The goal is to bring the issue to attention. Please make sure that the input string is trusted. If the occurrences don't seem to be valid, please feel free to ignore them.


## Bad practice

```python
cursor = connection.cursor()
cursor.execute("SELECT id FROM userdata WHERE Name =%s;" % name)  # Sensitve. Query constructed based on user's input
```

## Recommended

```python
cursor = connection.cursor()
# # Username is passed as a named parameter.
# Database will use the specified type and value of username when executing the query
cursor.execute("SELECT * FROM userdata WHERE Name = %s;", (name,))
```
