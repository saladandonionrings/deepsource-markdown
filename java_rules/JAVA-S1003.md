# Cookies must not be insecure
**ID:** `JAVA-S1003` | **Lien:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-S1003)

![Critical](https://img.shields.io/badge/severity-critical-red) ![Security](https://img.shields.io/badge/type-security-red)

A new cookie is created without the `Secure` flag set to `true` . The `Secure` flag is a browser directive that prevents the cookie from being transmitted over insecure connections ( `http://` ).


## Bad Practice

```java
Cookie cookie = new Cookie("userName",userName);
response.addCookie(cookie);
```

## Recommended
Always ensure that the `Secure` flag is set when creating the cookie.


```java
Cookie cookie = new Cookie("userName",userName);
cookie.setSecure(true); // Secure flag
cookie.setHttpOnly(true);
```
It is also possible to ensure that this is enforced through the servlet `web.xml` configuration, like so (this is specific to the Servlet 3.0 API):


```java
<web-app xmlns="http://java.sun.com/xml/ns/javaee" version="3.0">
[...]
<session-config>
 <cookie-config>
  <http-only>true</http-only>
  <secure>true</secure>
 </cookie-config>
</session-config>
</web-app>
```
