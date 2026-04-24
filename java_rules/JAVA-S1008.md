# Custom hashing algorithms must not be used
**ID:** `JAVA-S1008` | **Lien:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-S1008)

![Critical](https://img.shields.io/badge/severity-critical-red) ![Security](https://img.shields.io/badge/type-security-red)

Implementing a custom hashing algorithm can be error-prone and could allow for collision-based attacks on hashed data. Avoid implementing your own hash function, and use only trusted implementations.

NIST recommends the use of SHA-224, SHA-256, SHA-384, SHA-512, SHA-512/224, or SHA-512/256 when data needs to be hashed.


## Bad Practice
A custom hash function could have unforeseen vulnerabilities that reduce security and allow attackers to easily create collisions between hashed values.


```java
MyProprietaryMessageDigest extends MessageDigest {
    @Override
    protected byte[] engineDigest() {
        //Creativity is a bad idea
        return [...];
    }
    ...
}
```

## Recommended
Use an existing trusted hash algorithm that suits your security needs to generate hashes.


```java
MessageDigest sha256Digest = MessageDigest.getInstance("SHA256");
sha256Digest.update(password.getBytes());
```
