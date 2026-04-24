# Audit: Enabling JavaScript within a web view is a security risk
**ID:** `JAVA-A1029` | **Link:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-A1029)

![Critical](https://img.shields.io/badge/severity-critical-red)![Security](https://img.shields.io/badge/type-security-red)

Do not grant JavaScript execution permissions to a web view unless absolutely required.

Web views are containers for regular web pages, and as such have very similar considerations for security.

There is always the risk of a security flaw being found that would allow an attacker to execute malicious code within a web view.


## Bad Practice

```java
WebView webView = someView.findViewById(R.id.some_web_view);

// Only do this if you absolutely need it!
webView.getSettings().setJavaScriptEnabled(true);
```

## Recommended
Sometimes, executing JavaScript on a web view is unavoidable, and it is reasonable to enable its usage in such cases. However, take care to ensure that there is no way for an attacker to introduce their own scripts into the web view.

Use libraries such as OWASP's [ESAPI](https://owasp.org/www-project-enterprise-security-api/) to sanitize any input or output from the web view, and ensure that the user cannot directly control any data that is shared between the app and the web view.

