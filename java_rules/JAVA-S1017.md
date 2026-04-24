# Spring sessions must not be retained across user logins
**ID:** `JAVA-S1017` | **Link:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-S1017)

![Critical](https://img.shields.io/badge/severity-critical-red)![Security](https://img.shields.io/badge/type-security-red)

Spring can store the session of an authenticated user and preserve the session's state over time and even over different devices. This is called session fixation, and could allow an attacker to obtain information regarding a user's session. Such breaches of security could lead to more severe vulnerabilities later.

Always create a new session when a user logs in, and invalidate any existing, unused sessions which may become known to an attacker.

Spring's security configuration enables session fixation protection by default, but this protection can be removed by setting the session fixation policy to none.


## Bad Practice

```java
@Override
protected void configure(HttpSecurity http) throws Exception {
    http
    .sessionManagement()
    .sessionFixation().none(); // The same session will be used whenever the user logs in anywhere.
}
```

## Recommended
Spring's default session fixation policy is to create a new session and migrate the old session's attributes over to the new one. You can also choose to instead create a completely new session without any data from previous sessions.


```java
@Override
protected void configure(HttpSecurity http) throws Exception {
    http
    .sessionManagement()
    .sessionFixation().newSession(); // A new session will be created with nothing copied from previous sessions when a user logs in.

    // or

    // A new session will be created, the old session will be invalidated and attributes of the old session will be copied over.
    http
    .sessionManagement()
    .sessionFixation().migrateSession();
}
```
