# Audit: File can be modified or read by any user
**ID:** `JAVA-A1038` | **Lien:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-A1038)

![Critical](https://img.shields.io/badge/severity-critical-red) ![Security](https://img.shields.io/badge/type-security-red)

`File.setWritable()` is invoked in a way that allows all users to write to a file. This may expose a security vulnerability in the application through that file.

Avoid such permissive settings, as there is always a possibility of a malicious actor abusing them.


## Bad Practice
To allow any user to modify a file, one must invoke [ `File.setWritable(boolean, boolean)` ](https://docs.oracle.com/javase/7/docs/api/java/io/File.html#setWritable(boolean,%20boolean)) . This method's second argument controls whether write privileges are restricted to only the user who created the file (the user executing the program in many cases).

If set to false, any user will be able to write to the respective file.


```java
file.setWritable(true, false);
```

## Recommended
If multi-user access is not needed, consider using the single argument overload of [ `File.setWritable()` ](https://docs.oracle.com/javase/7/docs/api/java/io/File.html#setWritable(boolean)) instead to restrict access to the file.


```java
file.setWritable(true);
```
This can help reduce the attack surface by removing shared resources that can be manipulated.

