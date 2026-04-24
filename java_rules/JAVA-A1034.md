# Audit: User input should not directly be used in network calls
**ID:** `JAVA-A1034` | **Lien:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-A1034)

![Critical](https://img.shields.io/badge/severity-critical-red) ![Security](https://img.shields.io/badge/type-security-red)

Avoid using unsanitized data from sources like incoming requests or sockets in network calls.

This issue is raised when data from unsanitized sources such as a request parameter is directly used as a URL, or a part of a URL. Doing so can allow attackers to commit server-side request forgery (SSRF) attacks, where the attacker manipulates data in a way that causes a server itself to send sensitive data elsewhere, possibly even to the attacker's own server.

To mitigate this, care must be taken to avoid directly using user input as a URL, or as part of a URL when sending a web request.

This issue will be raised upon detection of invalid usage of libraries such as Java's [HttpClient]() and Apache's [Http]() [client]() libraries.


## Bad Practice
Consider this example where a servlet `GET` request is handled by sending another request elsewhere using the HttpClient API, introduced in Java 11.


```java
HttpClient client = HttpClient.newHttpClient();

@Override
void doGet(HttpServletRequest request, HttpServletResponse response) {
    URI uri = new URI(request.getParameter("dest"));
    // uri is used directly without any validation here!
    HttpRequest r = HttpRequest.newBuilder(uri).build();
    client.send(r, ...);

    // ...
}
```

## Recommended
If user input is needed to decide the destination of a request, and there are only a finite set of destinations that can be chosen, consider setting up a domain whitelist that can be chosen from through user input. This way, the user input cannot directly set the URL of the request, and will only be able to select it from a list of safe alternatives.


```java
List<String> destURLs = Arrays.asList(
    "some-url-1.com",
    "some-url-2.com",
    ...
);
// Make sure to handle parse exceptions gracefully!
int requestDestIndex = Integer.valueOf(request.getParameter("dest"));

if (requestDestIndex < 0 || requestDestIndex >= destURLs.size()) {
    // Invalid!
} else {
    // destURL is controlled by us, and cannot be modified by an attacker.
    String destURL = destURLs.get(requestDestIndex);
}
```
