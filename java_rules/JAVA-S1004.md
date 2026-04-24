# CBC and ECB modes are insecure
**ID:** `JAVA-S1004` | **Lien:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-S1004)

![Critical](https://img.shields.io/badge/severity-critical-red) ![Security](https://img.shields.io/badge/type-security-red)

`ECB` and `CBC` encryption modes are both known to be insecure due to a number of attacks that they make possible. As the default encryption mode in Java is ECB, this issue will be raised if only the algorithm is specified in a cipher transformation.

`CBC` encryption mode with any padding scheme is inherently susceptible to padding oracle attacks. An adversary could potentially decrypt the message if the system exposed the difference between plaintext with invalid padding or valid padding. The distinction between valid and invalid padding is usually revealed through distinct error messages being returned for each condition.

Similarly, there are a host of vulnerabilities associated with `ECB` mode which may be used to circumvent encryption.


## Bad Practice

```java
Cipher c1 = Cipher.getInstance("AES/CBC/PKCS5Padding");

Cipher c2 = Cipher.getInstance("AES"); // Java's default cipher provider sets mode to ECB if no mode is specified.
```

## Recommended
It is better to use an encryption mode which does not allow for such attacks and also incorporates integrity verification, such as Galois/Counter Mode (GCM). Additionally, always make sure to specify the full transformation string when specifying a cipher to instantiate.


```java
Cipher c = Cipher.getInstance("AES/GCM/NoPadding");
```
**Note:** Server-side code must ensure that data is verified for integrity before attempting to decrypt it. In the case of a padding oracle attack, the attacker would need to send data with compromised integrity as a part of the attack, and proper integrity verification via HMACs is generally enough to thwart such malicious traffic. Use an encryption mode which comes with HMAC integrity checks for best results.

