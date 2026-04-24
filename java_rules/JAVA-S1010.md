# NullCipher must not be used outside of tests
**ID:** `JAVA-S1010` | **Lien:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-S1010)

![Critical](https://img.shields.io/badge/severity-critical-red) ![Security](https://img.shields.io/badge/type-security-red)

`javax.crypto.NullCipher` is a cipher class intended for testing purposes. Using it outside of tests may leak important data in production.

`NullCipher` by design encrypts nothing, and returns the plain-text verbatim. It is not suitable for anything but tests, and is a bad sign when found anywhere else.


## Bad Practice

```java
Cipher c = NullCipher();
```

## Recommended
Remove usage of `NullCipher` in non-test files, and use a proper cipher implementation instead:


```java
Cipher c = Cipher.getInstance("AES/GCM/NoPadding");
```
