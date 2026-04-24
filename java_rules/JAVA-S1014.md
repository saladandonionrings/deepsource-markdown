# RSA keys must be at least 2048 bits long
**ID:** `JAVA-S1014` | **Lien:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-S1014)

![Critical](https://img.shields.io/badge/severity-critical-red) ![Security](https://img.shields.io/badge/type-security-red)

This code creates an RSA key pair with an insecure key size. This could reduce the security of the generated keys, allowing malicious actors to easily break encryption.


## Bad Practice
Using a key size less than 2048 bits (or 1024 for legacy applications alone) is insecure. As per the latest NIST advisory on good key lengths:

| Digital Signature Verification | RSA: 1024 <= len(n) < 2048 | Legacy-use | | Digital Signature Verification | RSA: len(n) >= 2048 | Acceptable |


```java
KeyPairGenerator kpg = KeyPairGenerator.getInstance("RSA");
kpg.initialize(512); // Insecure.
```

## Recommended
Always use a key size of at least 2048 bits to ensure proper security of your application.


```java
KeyPairGenerator kpg = KeyPairGenerator.getInstance("RSA");
kpg.initialize(2048);
```
