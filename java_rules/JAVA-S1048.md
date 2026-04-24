# JWTs should be checked for authenticity and integrity
**ID:** `JAVA-S1048` | **Link:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-S1048)

![Critical](https://img.shields.io/badge/severity-critical-red)![Security](https://img.shields.io/badge/type-security-red)

Always make sure to use only signed JWTs, and properly verify that a JWT's signature is valid before proceeding.

[`JWT`](https://en.wikipedia.org/wiki/JSON_Web_Token)s (or, as their specification calls them, "Jots") are a standard format for storing session data on the web. They are generally used in two forms:

* Plaintext ([`JWT`](https://javadoc.io/static/io.jsonwebtoken/jjwt-api/0.11.2/io/jsonwebtoken/Jwt.html)s when using the JJWT library)* Signed ([`JWS`](https://javadoc.io/static/io.jsonwebtoken/jjwt-api/0.11.2/io/jsonwebtoken/Jws.html)s in JJWT)
A plaintext `JWT` is not secure and may allow an attacker with control over the client side to easily forge session information sent to the server. Meanwhile, a `JWS` is more secure due to the fact that it also holds a signature derived from its contents. The signature can be generated securely, which means it is possible to check whether any tampering took place simply by comparing the included signature with the signature that would be generated with its current contents. If a mismatch occurs, the JWT can be considered invalid and rejected.

The [JJWT](https://javadoc.io/doc/io.jsonwebtoken/jjwt-api/latest/index.html) library allows the user to directly process JWTs through methods such as [`JwtParser.parse(String)`](https://javadoc.io/static/io.jsonwebtoken/jjwt-api/0.11.2/io/jsonwebtoken/JwtParser.html#parse(java.lang.String)), [`JwtParser.parseClaimsJwt(String)`](https://javadoc.io/static/io.jsonwebtoken/jjwt-api/0.11.2/io/jsonwebtoken/JwtParser.html#parseClaimsJwt(java.lang.String)) or [`JwtParser.parsePlaintextJwt(String)`](https://javadoc.io/static/io.jsonwebtoken/jjwt-api/0.11.2/io/jsonwebtoken/JwtParser.html#parsePlaintextJwt(java.lang.String)) and also to check whether they are properly signed, with methods such as [`JwtParser.parse(String, JwtHandler<T>)`](https://javadoc.io/static/io.jsonwebtoken/jjwt-api/0.11.2/io/jsonwebtoken/JwtParser.html#parse(java.lang.String,%20io.jsonwebtoken.JwtHandler)) (with the right arguments) and the JWS counterparts of the other methods above.

To allow the JJWT library to properly check signatures however, a matching signing key must be set while building a JWT parser. Only when both a signing key is set and the correct methods are used can JWTs actually be verified correctly. Simply using the `parse` method which takes only a single argument will not verify whether the signature is correct.


## Bad Practice
Using the single argument `parse`, `parseClaimsJwt` and `parsePlaintextJwt` methods will not check for signatures.


```java
Jwts.parserBuilder()
    .setSigningKey(signingKey)
    .build()
    .parse(jwt);

Jwts.parserBuilder()
    .setSigningKey(signingKey)
    .build()
    .parseClaimsJwt(jwt);

Jwts.parserBuilder()
    .setSigningKey(signingKey)
    .build()
    .parsePlaintextJwt(jwt);
```
Using the 2 argument `parse` method, which accepts a [`JwtHandler`](https://javadoc.io/doc/io.jsonwebtoken/jjwt-api/latest/io/jsonwebtoken/JwtHandler.html) instance also will not work if only the `onPlaintextJwt` method of the handler is overridden, as tokens with signatures will not be handled.


```java
Jwts.parserBuilder()
    .setSigningKey(signingKey).build()
    .parse(plaintextJwt, new JwtHandlerAdapter<Jwt<Header, String>>() {
        @Override
        public Jwt<Header, String> onPlaintextJwt(Jwt<Header, String> jwt) {
            return jwt; // Signed JWTs will never be handled with this code.
        }
    });
```

## Recommended
Always verify the signature by using either the `parseClaimsJws` and `parsePlaintextJws` methods or by overriding the `onPlaintextJws` or `onClaimsJws` methods of [`JwtHandlerAdapter`](https://javadoc.io/doc/io.jsonwebtoken/jjwt-api/latest/io/jsonwebtoken/JwtHandlerAdapter.html).


```java
Jwts.parserBuilder()
    .setSigningKey(signingKey).build()
    .parse(plaintextJwt, new JwtHandlerAdapter<Jws<String>>() {
        @Override
        public Jws<String> onPlaintextJws(Jws<String> jws) {
            return jws;
        }
    });
```
