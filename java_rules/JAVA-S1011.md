# Sockets must be secure
**ID:** `JAVA-S1011` | **Lien:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-S1011)

![Critical](https://img.shields.io/badge/severity-critical-red) ![Security](https://img.shields.io/badge/type-security-red)

`Socket` and `ServerSocket` do not implement TLS/SSL by default. Use `SSLSocket` / `SSLServerSocket` instead.

The socket factory types `javax.net.SocketFactory` and `javax.net.ServerSocketFactory` cannot be used to create secure client and server sockets. For that purpose, their subclasses, `SSLSocketFactory` and `SSLServerSocketFactory` must be used.


## Bad Practice

```java
Socket s = SocketFactory.getDefault().createSocket();

ServerSocket s2 = new ServerSocket(3434);
```

## Recommended

```java
Socket s = SSLSocketFactory.getDefault().createSocket();

ServerSocket s2 = SSLServerSocketFactory.getDefault().createSocket();
```
Beyond using an SSL socket, you need to make sure your use of `SSLSocketFactory` (or for server sockets, `SSLServerSocketFactory` ) does all the appropriate certificate validation checks to make sure you are not subject to man-in-the-middle attacks. Please read the [OWASP Transport Layer Protection Cheat Sheet](https://www.owasp.org/index.php/Transport_Layer_Protection_Cheat_Sheet) for details on how to do this correctly.

