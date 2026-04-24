# Audit: Web views should not have access to files
**ID:** `JAVA-A1028` | **Lien:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-A1028)

![Critical](https://img.shields.io/badge/severity-critical-red) ![Security](https://img.shields.io/badge/type-security-red)

Avoid granting file access privileges to web views.

Web views are containers for regular web pages, and as such have very similar considerations for security.

If a security flaw were discovered which could help an attacker inject malicious JavaScript code into the displayed web content, any malicious code could use this to access files from an affected device.


## Bad Practice
The `setAllowFileAccess()` and `setAllowContentAccess()` methods must not be called with `true` as an argument.


```java
WebView webView = someView.findViewById(R.id.some_web_view);

webView.getSettings().setAllowFileAccess(true);
webView.getSettings().setAllowContentAccess(true);
```

## Recommended
Disallow file/content access for such web views. If you require a web view to access files on the device, consider [binding a native interface](https://developer.android.com/guide/webapps/webview.html#BindingJavaScript) to the web view. JavaScript code within it would then need to interact with the safe interface controlled by you, preventing unauthorized access.

