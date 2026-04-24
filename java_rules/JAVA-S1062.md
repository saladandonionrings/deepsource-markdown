# SAML comment parsing should be disabled
**ID:** `JAVA-S1062` | **Link:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-S1062)

![Critical](https://img.shields.io/badge/severity-critical-red)![Security](https://img.shields.io/badge/type-security-red)

Parsing SAML comments should be disabled in applications using OpenSAML2.

SAML uses XML to exchange authentication response. Due to the way XML comments are parsed in various libraries, it is possible to alter the authentication response in such a way that allows an attacker to have unauthorized access to someone else's account. For this reason, applications relying on SAML should always configure the parser so that comments are always ignored.


## Bad Practice

```java
BasicParserPool basicPool = new BasicParserPool();
    basicPool.setIgnoreComments(false);
```

## Recommended
In OpenSAML 2.0, the default behavior in all `ParserPool` implementations is to ignore the comments. Just remove statements that explicitly enable comment parsing in the source.

