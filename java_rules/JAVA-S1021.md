# Insecure network protocols must not be used
**ID:** `JAVA-S1021` | **Link:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-S1021)

![Critical](https://img.shields.io/badge/severity-critical-red)![Security](https://img.shields.io/badge/type-security-red)

Insecure network protocols such as HTTP or FTP which do not make use of TLS/SSL can allow Man in the Middle (MitM) attacks to occur.

Use secure protocols whenever possible.

Clear-text protocols lack both encryption and verification features, and as such can allow attackers to easily intercept and/or manipulate data sent over them.

The risks of using such protocols are numerous, including but not limited to:

* Sensitive data leakage - any authentication data sent through an insecure protocol can be read by the attacker.* Phishing attacks - The attacker could pose as the server the client intended to talk to, or could impersonate a client in their communications to the server.* Client side attacks - The attacker could send the client malicious data or code to be executed.* Server side attacks - The attacker could modify requests from the client and attach malicious data or code that would be executed by the server.* Data loss- The attacker could corrupt the data in the request, leading to data loss or server side data corruption.
Additionally, HTTP as a protocol is deprecated by all major browsers.


## Bad Practice

```java
// These are from the Apache Commons Net library:

TelnetClient telnet = new TelnetClient();

FTPClient ftpClient = new FTPClient();

SMTPClient smtpClient = new SMTPClient();
```

## Recommended
Use SSH instead of Telnet. The [JSch](http://www.jcraft.com/jsch/) library is a good option to use here:


```java
JSch jsch = new JSch();
```
Instead of FTP, use SFTP, SCP or FTPS. JSch supports both SFTP and SCP protocols as well as SSH.

Apache provides a [client implementation for FTPS](https://commons.apache.org/proper/commons-net/apidocs/org/apache/commons/net/ftp/FTPSClient.html):


```java
FTPSClient client = new FTPSClient(implicit); // the connection can be implicit or explicit.
client.connect(...);
if (!implicit && client.execTLS()) {
    // ... Explicit mode.
} else {
    // ... Implicit mode.
}
```
Note that implicit FTP is deprecated. Prefer explicit mode unless the connection is required to be implicit ([here](https://www.ftptoday.com/blog/explicit-ftps-vs-implicit-ftps-what-you-need-to-know)'s an explanation of this).

Use Apache's [SMTPSClient](https://commons.apache.org/proper/commons-net/apidocs/index.html) to make secure SMTP connections:


```java
SMTPSClient client = new SMTPSClient(true);
client.connect(...);
if (!implicit && client.execTLS()) {
    // ... Explicit mode.
} else {
    // ... Implicit mode.
}
```
Use proper encryption when creating HTTPS connections.


## Exceptions
This issue will not be raised if a loopback connection (to `127.0.0.1` or `localhost`) is found to be made after creating the client.

Additionally, connections which are designed to operate within a private and secure environment such as a VPN may use unencrypted protocols.

While this is not ideal you may ignore this issue in such cases at your discretion.

