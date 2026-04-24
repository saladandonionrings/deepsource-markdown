# Insecure encryption algorithm detected
**ID:** `JAVA-S1012` | **Link:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-S1012)

![Critical](https://img.shields.io/badge/severity-critical-red)![Security](https://img.shields.io/badge/type-security-red)

This code was found to be using an insecure encryption algorithm. This could allow malicious actors to easily break encryption on the application, leading to data breaches or even hijacking of infrastructure.

A number of encryption algorithms exist which are widely supported, but are also deprecated due to their lack of security. For example, the following algorithms are insecure and their usage is not recommended:

* DES* Triple DES* ARCFOUR/RC4

## Bad Practice

```java
Cipher c = Cipher.getInstance("DES/ECB/PKCS5Padding");
c.init(Cipher.ENCRYPT_MODE, k, iv);
byte[] cipherText = c.doFinal(plainText);
```

## Recommended
Make sure to use modern, currently accepted encryption algorithms for all security sensitive operations.


```java
Cipher c = Cipher.getInstance("AES/GCM/NoPadding");
c.init(Cipher.ENCRYPT_MODE, k, iv);
byte[] cipherText = c.doFinal(plainText);
```
