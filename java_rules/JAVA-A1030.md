# Audit: Biometric authentication should always be used with a cryptographic object
**ID:** `JAVA-A1030` | **Link:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-A1030)

![Critical](https://img.shields.io/badge/severity-critical-red)![Security](https://img.shields.io/badge/type-security-red)

Biometric authentication should not be performed without an associated `CryptoObject` value.

Android allows one to perform [cryptography-related operations](https://developer.android.com/training/sign-in/biometric-auth#crypto) such as unlocking a keystore, or performing a signing operation through biometric authentication.

Note that Android's keystore API is separate from its biometric authentication API by design; both may be used independently of each other.

Biometric authentication is recommended for securely unlocking credentials stored in an Android keystore, because both biometric and keystore data are designed to be securely managed by Android.


## Bad Practice
Showing a biometric prompt without associating the authentication operation with some cryptographic operation is a bad idea; to give you an analogy -- it would be like trying to open a door by showing it a key instead of using the key to unlock it.

Now, it may be that a credential stored in the android certificate store does not require any user authentication to be unlocked. This is also suboptimal, since it means anyone could access such keys from the android keystore. To continue our analogy, it would be similar to leaving the door ajar for any person to enter.


```java
// Assume there is an instance of a biometric prompt stored in this variable...
biometricLoginButton.setOnClickListener(view -> {
    // This will show a biometric authentication prompt, but to what end?
    biometricPrompt.authenticate(promptInfo);
});
```
The snippet above would show a biometric authentication prompt when a particular button is tapped, but the prompt that is shown would be ineffective in securing the application.


## Recommended
Set up any keys to require user authentication by passing `true` to the [`KeyGenParameterSpec.Builder.setUserAuthenticationRequired()`](https://developer.android.com/reference/android/security/keystore/KeyGenParameterSpec.Builder#setUserAuthenticationRequired(boolean)) method.


```java
KeyGenerator keyGenerator = KeyGenerator.getInstance(KeyProperties.KEY_ALGORITHM_AES, "AndroidKeyStore");

// Ideally, do this only once, when your app is first started.
keyGenerator.init(new KeyGenParameterSpec.Builder(
                      "some_name",
                      KeyProperties.PURPOSE_ENCRYPT | KeyProperties.PURPOSE_DECRYPT)
                      .setBlockModes(KeyProperties.BLOCK_MODE_CBC)
                      .setEncryptionPaddings(KeyProperties.ENCRYPTION_PADDING_PKCS7)
                      // Require this key to be unlocked only via user authentication.
                      .setUserAuthenticationRequired(true)
                      // Require this key to only be unlocked with strong biometric authentication, with a timeout of 60 seconds.
                      .setUserAuthenticationParameters(60, KeyProperties.AUTH_BIOMETRIC_STRONG)
                      .build());
keyGenerator.generateKey();

KeyStore keyStore = KeyStore.getInstance("AndroidKeyStore");

keyStore.load(null);
SecretKey key = ((SecretKey)keyStore.getKey("some_key", null));
Cipher cipher = Cipher.getInstance("aes/gcm/nopadding");
cipher.init(Cipher.DECRYPT_MODE, key, new GCMParameterSpec(128, initializationVector));

// ...
```
When you show an authentication prompt, pass on the cipher as a `CryptoObject` value:


```java
biometricPrompt.authenticate(promptInfo, new BiometricPrompt.CryptoObject(cipher));
```
