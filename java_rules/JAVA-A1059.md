# SSLContext instances should not be constructed using "SSL"
**ID:** `JAVA-A1059` | **Link:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-A1059)

![Critical](https://img.shields.io/badge/severity-critical-red)![Security](https://img.shields.io/badge/type-security-red)

SSLContext should be initialized with `"TLS"` in order to use more recent TLS versions. If `SSL` is used instead as the protocol string, the implementation will default to an older, insecure version of TLS or SSL.

SSL is the original protocol used to encrypt secure HTTP traffic (like when `https://` is used). However, its latest version (SSL 3.0) was deprecated in 2015 by the IETF. Continued use of SSL is not recommended because of the security flaws present in its implementation.

It is better to switch to a modern protocol such as TLS 1.3.


## Bad Practice

```java
SSLContext context = SSLContext.getInstance("SSL");
```

## Recommended
If your infrastructure supports it, consider using TLS 1.3, the latest version of TLS.


```java
SSLContext context = SSLContext.getInstance("TLSv1.3");
```
Otherwise, set the string used to just `TLS` in order to (at least) make use of the latest TLS version supported by your infrastructure.

