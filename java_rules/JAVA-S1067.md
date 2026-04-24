# Deprecated `HttpClient` implementations should not be used
**ID:** `JAVA-S1067` | **Link:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-S1067)

![Critical](https://img.shields.io/badge/severity-critical-red)![Security](https://img.shields.io/badge/type-security-red)

The `DefaultHttpClient` class has been deprecated since Apache httpclient library version `4.3`. Avoid using it, as it does not make use of the latest TLS standard, leading to the possibility of a MiTM (Man in The Middle) attack.


## Bad Practice

```java
HttpClient client = new DefaultHttpClient();
```

## Recommended
There are a number of alternatives you can use instead.

Set the `http.protocols` system property to take advantage of the latest TLS version:


```java
java ... -Dhttps.protocols=TLSv1.2,TLSv1.3
```
Now, you can make use of one of the following alternatives to create a suitable `HttpClient`.

* [`HttpClients.createSystem()`](https://hc.apache.org/httpcomponents-client-5.2.x/current/httpclient5/apidocs/org/apache/hc/client5/http/impl/classic/HttpClients.html#createSystem--)

```java
HttpClient client = HttpClients.createSystem();
```
* [`HttpClientBuilder`](https://hc.apache.org/httpcomponents-client-5.2.x/current/httpclient5/apidocs/org/apache/hc/client5/http/impl/classic/HttpClientBuilder.html)

```java
HttpClient client = HttpClientBuilder.create().useSystemProperties().build();
```
