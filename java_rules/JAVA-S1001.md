# Servlet does not sanitize path names from HTTP requests
**ID:** `JAVA-S1001` | **Lien:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-S1001)

![Critical](https://img.shields.io/badge/severity-critical-red) ![Security](https://img.shields.io/badge/type-security-red)

This servlet uses an HTTP request parameter to construct a path. While this action may mean to access only one directory in the server's file system, it does not properly neutralize sequences such as `".."` that can resolve to a location that is outside that directory.

Consider a servlet that takes `GET` or `POST` requests in the following form:


```java
http://example.com.br/get-files?file=report.pdf
```
If the servlet processes the request by simply appending the file name to a predefined path, accessing the file system through that path will be susceptible to relative path modification attacks:


## Bad Practice

```java
String BASE_PATH = "/home/users/";
String userName = ...; // From a database, possibly.
// Expands to: "/home/users/username/filename"
String filePath = BASE_PATH + userName + "/" + request.getParameter(REQUEST_PARAMETER);
// ...
```
`REQUEST_PARAMETER` can be used to access files from other usernames by using a relative path:


```java
http://example.com.br/get-files?file=../some_other_username/filename.txt
```
The requested file name will be appended and interpreted as the following malicious path:


```java
/home/users/username/../some_other_username/filename.txt
```
Or, canonically:


```java
/home/users/some_other_username/filename.txt
```
This is a serious security risk since it allows users to steal others' information.


## Recommended
There are multiple ways to resolve this. For example, efforts could be made to:

* Sanitize url parameters to ensure they do not contain malicious inputs
* Assign directory permissions of users in such a way that this type of attack cannot occur
* Check the user id when reading data related to that id

