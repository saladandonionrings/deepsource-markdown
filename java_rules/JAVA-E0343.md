# Method attempts to access a result set field with index 0
**ID:** `JAVA-E0343` | **Lien:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-E0343)

![Critical](https://img.shields.io/badge/severity-critical-red) ![Bug Risk](https://img.shields.io/badge/type-bug_risk-green)

A call to a `getXXX` or `updateXXX` method of a result set was made where the field index is `0` . As `ResultSet` fields start at index `1` , this is always a mistake.

Using a 0 index with `ResultSet` 's getter and update methods will only trigger an `SQLException` .


## Bad Practice

```java
Connection c = DriverManager.getConnection(...);

Statement s = conn.createStatement();
s.execute("SELECT userName, isWin FROM users WHERE uid = 'someuser';");
ResultSet r = s.getResultSet();

String userName = r.getString(0); // This will fail.
```

## Recommended

```java
String userName = r.getString(1);
```
Consider using column names instead of indices to avoid such mistakes in the future. Another possible way to mitigate this issue would be to have integer constants labelled with the respective column names.


```java
int USER = 1;

String userName = r.getString(USER);

// Or:

String userName1 = r.getString("user");
```
