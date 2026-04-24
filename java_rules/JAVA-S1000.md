# Overly permissive CORS policies are a security risk
**ID:** `JAVA-S1000` | **Lien:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-S1000)

![Critical](https://img.shields.io/badge/severity-critical-red) ![Security](https://img.shields.io/badge/type-security-red)

An overly permissive CORS policy can allow malicious actors to retrieve sensitive data from your own servers through the client.

Prior to HTML5, Web browsers enforced the Same Origin Policy which ensures that in order for JavaScript to access the contents of a Web page, both the JavaScript and the Web page must originate from the same domain. Without the Same Origin Policy, a malicious website could serve up JavaScript that loads sensitive information from other websites using a client's credentials, and communicate this information back to the attacker.

HTML5 makes it possible for JavaScript to access data across domains if the `Access-Control-Allow-Origin` HTTP header is defined. With this header, a web server defines which other domains are allowed to access its domain using cross-origin requests. However, caution should be taken when defining the header because an overly permissive CORS policy will allow a malicious application to communicate with the victim application in an inappropriate way, leading to spoofing, data theft, relay and other attacks.


## Bad Practice

```java
response.addHeader("Access-Control-Allow-Origin", "*");
```

## Recommended
Though it is not possible to specify multiple origins directly using this header, you can use the request's `Origin` header along with a domain whitelist to accomplish this:


```java
List<String> whitelistedDomains = Arrays.asList("some.trusted.domain", "some.other-trusted.domain", ...);

// CORS requests always carry an Origin header specifying the domain of the page that initiated the request to this server.
String origin = request.getHeader("Origin");

// If the origin domain is safe, we could allow such cross origin requests to go through.
if (whilelistedDomains.contains(origin))
    response.addHeader("Access-Control-Allow-Origin", origin);
```
