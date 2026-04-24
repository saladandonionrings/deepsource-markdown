# Basic authorization is a security risk
**ID:** `JAVA-S1019` | **Lien:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-S1019)

![Major](https://img.shields.io/badge/severity-major-orange) ![Security](https://img.shields.io/badge/type-security-red)

Basic authorization only encodes the user name and password in base-64 before sending it to the server, which is just a step above sending the data as plain-text.


## Bad Practice

```java
import org.apache.http.client.methods.HttpPost;

// ...

// Using HttpPost from Apache HttpClient
// Apache provides a Base64 encoder class for convenience.
String encoding = Base64Encoder.encode ("login:passwd");
HttpPost post = new HttpPost(url);
post.setHeader("Authorization", "Basic " + encoding);

// or

import java.net.HttpUrlConnection;

// ...

// Using HttpURLConnection
String encoding = Base64.getEncoder().encodeToString(("login:passwd").getBytes("UTF-8"));
HttpURLConnection conn = (HttpURLConnection) url.openConnection();
conn.setRequestMethod("POST");
conn.setDoOutput(true);
conn.setRequestProperty("Authorization", "Basic " + encoding);
```

## Recommended
Do not use basic authorization to authenticate users. While it may take more effort to securely send authentication data to the server, it will be beneficial in the long run.

If it is not possible to move away from it, ensure that any such authentication is done only over HTTPS.

