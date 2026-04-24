# RSA without padding is insecure
**ID:** `JAVA-S1013` | **Link:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-S1013)

![Critical](https://img.shields.io/badge/severity-critical-red)![Security](https://img.shields.io/badge/type-security-red)

This code creates a `javax.crypto.Cipher` instance using the RSA algorithm with no padding. This is a security risk, and must be avoided.

Without using a proper padding scheme to "armor" the encrypted ciphertext, RSA encryption can be insecure and may be easily broken.

Secure RSA encryption schemes pad or "armor" the plaintext with securely randomized data to ensure that each plaintext is unique before encryption. Without adding the extra random data, RSA becomes a more basic version known as Textbook RSA, that takes on two undesirable properties:

This can greatly reduce the security provided by encryption and must be avoided.


## Bad Practice

```java
Cipher.getInstance("RSA/NONE/NoPadding")
```

## Recommended
Consider using one of the NIST approved OAEP (Optimal Assymmetric Encryption Padding) padding schemes:


```java
Cipher.getInstance("RSA/ECB/OAEPWithMD5AndMGF1Padding")
```
For more information regarding appropriate padding schemes to use, consult the Java security standard algorithm names specification provided by Oracle.

