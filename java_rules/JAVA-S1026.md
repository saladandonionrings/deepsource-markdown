# LDAP object deserialization is a security risk
**ID:** `JAVA-S1026` | **Link:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-S1026)

![Critical](https://img.shields.io/badge/severity-critical-red)![Security](https://img.shields.io/badge/type-security-red)

[LDAP](https://en.wikipedia.org/wiki/Lightweight_Directory_Access_Protocol) (Lightweight Directory Access Protocol) search queries should not allow objects found to be deserialized.

This issue is raised when an LDAP [`SearchControls`](https://docs.oracle.com/javase/7/docs/api/javax/naming/directory/SearchControls.html) object is configured to allow deserialization of objects retrieved via a search query.

[JNDI](https://www.oracle.com/java/technologies/jndi-overview.html) is a protocol that allows Java classes from remote locations to be downloaded and added into an application's classpath at runtime.

LDAP is a protocol that allows data including Java classes to be stored as hierarchical key-value pairs.

These two protocols can be used together; Java classes or objects can be stored as LDAP objects and retrieved through search queries. However, because this works through Java's own object serialization protocols, an attacker could create a malicious LDAP entry which would enable a variety of attacks. For example, such an LDAP entry could be created which references a class present on the attacker's server. When deserialized, the attacker's LDAP server would be queried for the specified class file, and once the class is loaded into the JVM, it would be possible for the class to execute any malicious code as part of its static initialization. Of course, such an attack is predicated on controlling a number of factors. It is important to be aware and reduce the attack surface available to malicious actors.


## Bad Practice

```java
SearchControls ctls = new SearchControls(
    scope,
    countLimit,
    timeLimit,
    attributes,
    true,       // This argument should be set to false to avoid deserializing any entries found through a search query.
    derefLinks
);


ctls.setReturningObjFlag(true); // This does the same thing.
```
The returning object flag, as it is called, is set by default to `false`. Usually, when values are to be retrieved using an LDAP search, only their names and attributes are retrieved. However, if this flag is set to `true` and a Java serialized object was stored in the entry, Java will also try to deserialize and return an object version of it.


## Recommended
Set the returning object flag to `false` when performing LDAP search queries, either with the `SearchControls` constructor or by calling [`setReturningObjFlag()`](https://docs.oracle.com/javase/7/docs/api/javax/naming/directory/SearchControls.html#setReturningObjFlag(boolean)) with `false`.


```java
SearchControls ctls = new SearchControls(
    scope,
    countLimit,
    timeLimit,
    attributes,
    false,
    derefLinks
);

// or...

ctls.setReturningObjFlag(false);
```
