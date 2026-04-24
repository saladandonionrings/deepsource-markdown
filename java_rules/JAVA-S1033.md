# SMTP configurations should check SSL certificates for authenticity
**ID:** `JAVA-S1033` | **Lien:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-S1033)

![Critical](https://img.shields.io/badge/severity-critical-red) ![Security](https://img.shields.io/badge/type-security-red)

JavaMail SMTP configurations should have secure SSL configurations.

Java's SMTP API, JavaMail is widely used to send emails. Similarly to normal HTTP communication, it is possible to use SSL or TLS based encryption to ensure security. However, unless host-specific certificate authenticity is specifically checked for, it will be possible for a man-in-the-middle attack to occur.

It is recommended to explicitly enable SSL/TLS certificate checking to ensure connections are properly secured.


## Bad Practice
In this example, SMTP authentication is enabled for a JavaMail [session](https://javaee.github.io/javamail/docs/api/javax/mail/Session.html) , but certificate checking is not.


```java
Properties properties = PropertiesUtil.getSystemProperties();
properties.put("mail.transport.protocol", "protocol");
properties.put("mail.smtp.host", "hostname");
properties.put("mail.smtp.socketFactory.class", "classname");
properties.put("mail.smtp.auth", "true");

Authenticator authenticator = ...; // Create an authenticator implementation.
Session session = Session.getInstance(properties, authenticator);
```

## Recommended
Set the `"mail.smtp.ssl.checkserveridentity"` property to `"true"` to ensure that certificates are properly verified.


```java
properties.put("mail.smtp.ssl.checkserveridentity", "true");
```
