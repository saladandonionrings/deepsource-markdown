# Spring password storage must use a strong hashing function
**ID:** `JAVA-S1018` | **Link:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-S1018)

![Critical](https://img.shields.io/badge/severity-critical-red)![Security](https://img.shields.io/badge/type-security-red)

This Spring security configuration appears to store passwords in plaintext or hashed with a weak hashing algorithm. This could allow an attacker to easily steal user login information.

Configure Spring to store passwords securely.

Spring allows for great flexibility when configuring how user information is stored in the database.

User passwords in particular are a liability when not stored properly; they must be hashed and salted before storage.

Ideally, a strong hash algorithm is:

* Not vulnerable to brute force attacks* Not vulnerable to collision attacks* Not vulnerable to rainbow table attacks; this is achieved by salting the password with random data.
This issue is raised when either no password encoder, or one of the following weak/deprecated password encoders is used:

* [`StandardPasswordEncoder`](https://docs.spring.io/spring-security/site/docs/current/api/org/springframework/security/crypto/password/StandardPasswordEncoder.html)- Though it claims to be standard, even Spring has deprecated its use.* [`NoOpPasswordEncoder`](https://docs.spring.io/spring-security/site/docs/current/api/org/springframework/security/crypto/password/NoOpPasswordEncoder.html)- Using this is the same as not using a password encoder. While it is permissible for testing purposes, it must never be used in production.* [`MessageDigestPasswordEncoder`](https://docs.spring.io/spring-security/site/docs/current/api/org/springframework/security/crypto/password/MessageDigestPasswordEncoder.html)- This encoder is insecure, as one could couple it with an insecure message digest algorithm.* [`Md4PasswordEncoder`](https://docs.spring.io/spring-security/site/docs/current/api/org/springframework/security/crypto/password/Md4PasswordEncoder.html)- MD4's security as a hash function is severely compromised.* [`LdapShaPasswordEncoder`](https://docs.spring.io/spring-security/site/docs/current/api/org/springframework/security/crypto/password/LdapShaPasswordEncoder.html)- This encoder is insecure for a number of reasons.

## Bad Practice

```java
@Autowired
public void configureGlobal(AuthenticationManagerBuilder auth, DataSource dataSource) throws Exception {
  auth.jdbcAuthentication()
    .dataSource(dataSource)
    .usersByUsernameQuery("SELECT * FROM users WHERE username = ?")
    .passwordEncoder(new StandardPasswordEncoder()); // StandardPasswordEncoder is not secure.

  // OR
  auth.jdbcAuthentication()
    .dataSource(dataSource)
    .usersByUsernameQuery("SELECT * FROM users WHERE username = ?"); // If no encoder is used, the password is stored as plain-text.

  // OR
  auth.userDetailsService(...); // Again, the password is stored as plain-text.
  // OR
  auth.userDetailsService(...).passwordEncoder(new LdapShaPasswordEncoder()); // Insecure.
}
```

## Recommended
Use [`DelegatingPassswordEncoder`](https://docs.spring.io/spring-security/site/docs/current/api/org/springframework/security/crypto/password/DelegatingPasswordEncoder.html) with any of these encoders:

* [`Argon2PasswordEncoder`](https://docs.spring.io/spring-security/site/docs/current/api/org/springframework/security/crypto/argon2/Argon2PasswordEncoder.html)* [`BCryptPasswordEncoder`](https://docs.spring.io/spring-security/site/docs/current/api/org/springframework/security/crypto/bcrypt/BCryptPasswordEncoder.html)* [`Pbkdf2PasswordEncoder`](https://docs.spring.io/spring-security/site/docs/current/api/org/springframework/security/crypto/password/Pbkdf2PasswordEncoder.html)* [`SCryptPasswordEncoder`](https://docs.spring.io/spring-security/site/docs/current/api/org/springframework/security/crypto/scrypt/SCryptPasswordEncoder.html)- While Scrypt is quite secure, there are[some concerns regarding its use with passwords](https://blog.ircmaxell.com/2014/03/why-i-dont-recommend-scrypt.html).
Here's an example using `BCryptPasswordEncoder`:


```java
@Autowired
public void configureGlobal(AuthenticationManagerBuilder auth, DataSource dataSource) throws Exception {
  auth.jdbcAuthentication()
    .dataSource(dataSource)
    .usersByUsernameQuery("Select * from users where username=?")
    .passwordEncoder(new BCryptPasswordEncoder());
}
```
