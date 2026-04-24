# Insecure hash algorithm usage with passwords detected
**ID:** `JAVA-S1006` | **Link:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-S1006)

![Critical](https://img.shields.io/badge/severity-critical-red)![Security](https://img.shields.io/badge/type-security-red)

The MD* family of hashing algorithms (MD2, MD4 and MD5), as well as SHA-1 are cryptographically insecure and are very susceptible to collision attacks. These algorithms must not be used to hash passwords or cryptographically significant values.

MD5 in particular suffers from a number of collision attacks, which render it unsuitable as a password hash function:

... The security of the MD5 hash function is severely compromised. A collision attack exists that can find collisions within seconds on a computer with a 2.6 GHz Pentium 4 processor (complexity of 224.1).Further, there is also a chosen-prefix collision attack that can produce a collision for two inputs with specified prefixes within hours, using off-the-shelf computing hardware (complexity 239). ...

* Wikipedia: MD5 - Security
Similarly, SHA-1 is not safe for use as a password or signature verification algorithm.

SHA-1 for digital signature generation: SHA-1 may only be used for digital signature generation where specifically allowed by NIST protocol-specific guidance. For all other applications, SHA-1 shall not be used for digital signature generation. SHA-1 for digital signature verification: For digital signature verification, SHA-1 is allowed for legacy-use. ...SHA-224, SHA-256, SHA-384, SHA-512, SHA-512/224, and SHA-512/256: The use of these hash functions is acceptable for all hash function applications.

* NIST: Transitioning the Use of Cryptographic Algorithms and Key Lengths (p15)
Passwords must be hashed only using specific password hashing algorithms such as `PBKDF2`. This is because such algorithms are tailored to the specific use case of ensuring an attacker cannot easily perform a brute force attack to figure out the password:

... The main idea of a PBKDF is to slow dictionary or brute force attacks on the passwords by increasing the time needed to test each password. An attacker with a list of likely passwords can evaluate the PBKDF using the known iteration counter and the salt. Since an attacker has to spend a significant amount of computing time for each try, it becomes harder to apply the dictionary or brute force attacks. ...

* NIST: Recommendation for Password Based Key Derivation, (p12)

## Bad Practice

```java
MessageDigest md = MessageDigest.getInstance("SHA1");
String password = "secret";
Byte[] output = md.digest(pasword.getBytes());
```

## Recommended
In general, there are a number of choices for hash functions that do not have such glaring collision vulnerabilities:

... SHA-224, SHA-256, SHA-384, SHA-512, SHA-512/224, and SHA-512/256: The use of these hash functions is acceptable for all hash function applications. ...

* NIST: Transitioning the Use of Cryptographic Algorithms and Key Lengths (p15)
`PBKDF2` is the recommended algorithm to use when hashing passwords.

Here is an example of its usage, in Java 8 and above:


```java
public static byte[] getEncryptedPassword(String password, byte[] salt) throws NoSuchAlgorithmException, InvalidKeySpecException {
    KeySpec spec = new PBEKeySpec(password.toCharArray(), salt, 4096, 256 * 8);
    SecretKeyFactory f = SecretKeyFactory.getInstance("PBKDF2WithHmacSHA256");
    return f.generateSecret(spec).getEncoded();
}
```
If the `javax.crypto` package cannot be used, or you are using an older version of Java, you could use the BouncyCastle library:


```java
public static byte[] getEncryptedPassword(String password, byte[] salt) throws NoSuchAlgorithmException, InvalidKeySpecException {
    PKCS5S2ParametersGenerator gen = new PKCS5S2ParametersGenerator(new SHA256Digest());
    gen.init(password.getBytes("UTF-8"), salt.getBytes(), 4096);
    return ((KeyParameter) gen.generateDerivedParameters(256)).getKey();
}
```
