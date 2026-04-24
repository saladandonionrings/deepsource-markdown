# A TrustManager/HostnameVerifier that accepts all certificates is a security risk
**ID:** `JAVA-S1002` | **Link:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-S1002)

![Critical](https://img.shields.io/badge/severity-critical-red)![Security](https://img.shields.io/badge/type-security-red)

Java uses the `TrustManager` and `HostnameVerifier` APIs to verify that an SSL connection is properly secured. If a bad implementation of these classes is used, it may allow for malicious hosts to connect to the application.


## Bad Practice
This `TrustManager` implementation will trust any certificate it is given, meaning even untrusted certificates from a malicious actor could be used.


```java
class TrustAllManager implements X509TrustManager {

    @Override
    public void checkClientTrusted(X509Certificate[] x509Certificates, String s) throws CertificateException {
        //Trust any client connecting (no certificate validation)
    }

    @Override
    public void checkServerTrusted(X509Certificate[] x509Certificates, String s) throws CertificateException {
        //Trust any remote server (no certificate validation)
    }

    @Override
    public X509Certificate[] getAcceptedIssuers() {
        return null;
    }
}
```
The `HostnameVerifier` interface is used as the final check for the authenticity of a remote connection when all other methods of URL verification have failed. When default certificate validation fails, it is likely that the resulting connection will be insecure and must not be used.

Situations where this may occur include:

* When the URL used to connect to the remote host isn't the same as the one on that host's certificate* When the remote host server is misconfigured with the wrong certificate.
This implementation of it will trust any hostname it is given:


```java
public class AllHosts implements HostnameVerifier {
    public boolean verify(final String hostname, final SSLSession session) {
        return true;
    }
}
```

## Recommended
To create a `TrustManager` without vulnerabilities, using the `TrustManagerFactory` API to create a `TrustManager` using a keystore is recommended:


```java
KeyStore ks = //Load keystore containing the certificates trusted

SSLContext sc = SSLContext.getInstance("TLS");

TrustManagerFactory tmf = TrustManagerFactory.getInstance("SunX509");
tmf.init(ks);

sc.init(kmf.getKeyManagers(), tmf.getTrustManagers(),null);
```
When HostnameVerifier needs to be overridden, it is usually because the default certificate based host validation has failed, and the cause is likely remote server side. If possible, consider fixing the problem on the remote server side to obviate the need to handle such issues through `HostnameVerifier`. Consider the alternative only if there is no scope to fix the problem at the source.

In the general case, consider using an implementation of `HostnameVerifier` that trusts nothing:


```java
public class AllHosts implements HostnameVerifier {
    public boolean verify(final String hostname, final SSLSession session) {
        return false;
    }
}
```
