# Blowfish keys must be at least 128 bits long
**ID:** `JAVA-S1015` | **Link:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-S1015)

![Critical](https://img.shields.io/badge/severity-critical-red)![Security](https://img.shields.io/badge/type-security-red)

The Blowfish cipher supports key sizes from 32 bits to 448 bits. A small key size makes the ciphertext vulnerable to brute force attacks.

At least 128 bits of entropy should be used when generating Blowfish keys.

The blowfish cipher is a reliable symmetric block-based encryption algorithm with good performance and security for plaintext smaller than 4 Gigabytes in size. This size limitation stems from the smaller block size (64 bits), with larger plain-texts suffering from the possibility of a birthday attack reducing the cipher's security. At lower-key sizes, the security of the blowfish cipher degrades due to the increased chance of a brute force attack succeeding.

It is thus recommended that the key be at minimum 128 bits long.


## Bad Practice

```java
KeyGenerator kg = KeyGenerator.getInstance("Blowfish");
kg.initialize(64); // Insecure.
```

## Recommended
Always use a key size of at least 2048 bits to ensure proper security of your application.


```java
KeyGenerator kg = KeyGenerator.getInstance("Blowfish");
kg.initialize(128);
```
