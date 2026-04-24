# Database password field is empty
**ID:** `JAVA-S0014` | **Lien:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-S0014)

![Critical](https://img.shields.io/badge/severity-critical-red) ![Security](https://img.shields.io/badge/type-security-red)

The password field for this database connection is empty.

This code creates a database connection using a blank or empty password. This indicates that the database is not protected by a password. Because the only information required to access such a database is its address, any information stored in it is safe only due to the obscurity of the database's address.


## Examples

## Bad Practice

```java
Connection conn = DriverManager.getConnection("jdbc:derby:memory:myDB;create=true", "AppLogin", "");
```
Reliance on security by obscurity is heavily discouraged as it provides a convenient way for malicious actors to gain control of private data.


## Recommended

```java
String secretPassword = ...;

Connection conn = DriverManager.getConnection("jdbc:derby:memory:myDB;create=true", "AppLogin", secretPassword);
```
