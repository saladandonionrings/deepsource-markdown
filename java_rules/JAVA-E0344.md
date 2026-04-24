# Method attempts to set a prepared statement parameter with index 0
**ID:** `JAVA-E0344` | **Lien:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-E0344)

![Critical](https://img.shields.io/badge/severity-critical-red) ![Bug Risk](https://img.shields.io/badge/type-bug_risk-green)

A call to a `setXXX` method of a prepared statement was made where the parameter index is `0` . As SQL parameter indexes start at index `1` , this is always a mistake.

Using a 0 index with `PreparedStatement` 's setter methods will only trigger an `SQLException` .


## Bad Practice

```java
Connection c = DriverManager.getConnection(...);

PreparedStatement s = conn.prepareStatement("SELECT userName, isWin FROM users WHERE uid = ?;");
s.setString(0, "User"); // This will fail.
s.execute();
```

## Recommended

```java
s.setString(1, "User");
```
Consider using column names instead of indices to avoid such mistakes in the future. Another possible way to mitigate this issue would be to have integer constants labelled with the respective column names.


```java
int USER = 1;

s.setString(USER, "User");

// Or:

s.setString("user", "User");
```
