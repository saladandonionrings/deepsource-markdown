# `getRequestSessionId` should not be used
**ID:** `JAVA-S1063` | **Lien:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-S1063)

![Critical](https://img.shields.io/badge/severity-critical-red) ![Security](https://img.shields.io/badge/type-security-red)

The session ID returned by `getRequestSessionId` isn't necessarily the one belonging to the current user.

As per the [Oracle Java Docs](https://docs.oracle.com/javaee/6/api/javax/servlet/http/HttpServletRequest.html) , `getRequestSessionId` returns the session ID that is specified by the client through cookies or URL parameters.

Since the client has full control over the session ID returned from `getRequestedSessionId` , a malicious attacker could easily gain unauthorized access to someone else's account if they supply an active session ID that belongs to someone else.


## Bad Practice

```java
public Response handleRequest(HttpServletRequest request) {
    val sessionID = request.getRequestedSessionId();
    // Do something that requires authorization using the sessionID.
    doAuthorizedTask(sessionID);

    // ...rest of the code
}
```

## Recommended
Do not use user supplied session IDs for authorization purposes. Store it in the server or a database and query it as required.

