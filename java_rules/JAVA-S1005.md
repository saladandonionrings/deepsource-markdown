# Cipher does not support integrity verification
**ID:** `JAVA-S1005` | **Link:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-S1005)

![Critical](https://img.shields.io/badge/severity-critical-red)![Security](https://img.shields.io/badge/type-security-red)

The ciphertext produced is susceptible to alteration by an adversary. This means that the cipher provides no way to detect that the data has been tampered with. If the ciphertext can be controlled by an attacker, it could be altered without detection.

The solution is to use a cipher that includes a Hash-based Message Authentication Code (HMAC) to sign the data. Combining an HMAC function with the existing cipher is prone to error. Specifically, it is always recommended that you be able to verify the HMAC first, and only if the data is unmodified, do you then perform any cryptographic functions on the data. Essentially, it is best to provide an encrypted payload with the HMAC of the encrypted data which can be used to verify integrity before decryption.

The following modes are vulnerable because they don't provide an HMAC:

* CBC* OFB* CTR* ECB

## Bad Practice

```java
Cipher c = Cipher.getInstance("AES/CBC/PKCS5Padding");
c.init(Cipher.ENCRYPT_MODE, k, iv);
byte[] cipherText = c.doFinal(plainText);
```

## Recommended
Use a cipher mode that supports HMACs for verification by default, such as GCM:


```java
Cipher c = Cipher.getInstance("AES/GCM/NoPadding");
c.init(Cipher.ENCRYPT_MODE, k, iv);
byte[] cipherText = c.doFinal(plainText);
```
