# Audit: Including request data within HTML response strings may lead to XSS attacks
**ID:** `JAVA-A1035` | **Link:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-A1035)

![Critical](https://img.shields.io/badge/severity-critical-red)![Security](https://img.shields.io/badge/type-security-red)

Avoid directly including request data within HTML, as this may lead to a cross-site-scripting vulnerability.

When unsanitized data from a HTTP request is used to create a HTML page to be sent back in the response, an attacker may be able to include malicious scripts or links within the response by controlling the data in the request.


## Bad Practice

```java
String userName = req.getParameter("user");

String template = "<p>Hi, %s</p>";

String renderedPage = String.format(template, userName);

PrintWriter writer = resp.getWriter();

response.setStatus(200);

writer.print(renderedPage);
writer.flush();
```
Here, if the request parameter `user` was `"Ralph"`, the data in the response would read as:


```java
<p>Hi, Ralph!</p>
```
Now, what if `name` contained some JavaScript code in a `<script>` tag?


```java
<script>alert("hacked")</script>Ralph
```
If a request was sent with this data, the output in the response would look like this:


```java
<p>Hi, <script>alert("hacked")</script>Ralph!</p>
```
When the user's browser displays the result of the response, an alert would pop up that said `"hacked"`.

Obviously, this is just a simple example of what is possible. A more dangerous attack may involve malicious UI elements or popups that look similar to the real website, but are used only to gain access to account information.


## Recommended
Make use of tools such as OWASP's ESAPI or [Java HTML Sanitizer](https://owasp.org/www-project-java-html-sanitizer/) libraries to sanitize untrusted input data before using that data within a user-facing response.

Here is an example of using the OWASP HTML Sanitizer library, adapted from OWASP's XSS cheat sheet:


```java
import org.owasp.html.Sanitizers;
import org.owasp.html.PolicyFactory;

// ...

PolicyFactory sanitizer = Sanitizers.FORMATTING.and(Sanitizers.BLOCKS);
String sanitizedText = sanitizer.sanitize(userName);

String safeRenderedText = String.format(template, sanitizedText);
```
Note that the location of text to be rendered matters greatly; escape sequences that are valid within a HTML attribute may not be valid in JavaScript code for example. For this reason, the ESAPI library provides a [variety of different encoders](https://javadoc.io/static/org.owasp.encoder/encoder/1.2.3/org/owasp/encoder/Encoders.html), and context specific encoding methods within the [Encode](https://javadoc.io/static/org.owasp.encoder/encoder/1.2.3/org/owasp/encoder/Encode.html) class for various use cases:


```java
String htmlSafe = Encode.forHtml(userName);

String htmlAttrSafe = Encode.forJavaScript(userName);
```
