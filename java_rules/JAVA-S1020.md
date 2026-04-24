# LDAP connections should be authenticated
**ID:** `JAVA-S1020` | **Link:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-S1020)

![Critical](https://img.shields.io/badge/severity-critical-red)![Security](https://img.shields.io/badge/type-security-red)

A JNDI LDAP configuration was found where authentication was disabled.

This is highly discouraged, as it means the LDAP binding is accessible to any client that has its address.

Simple authentication in LDAP can be used with three different mechanisms:

* Anonymous - Both username and password are not provided in the bind request.* Unauthenticated - No password is provided in the bind request.* Name/Password Authentication - A username and a password are provided in the bind request.
Anonymous binds and unauthenticated binds allow access to information in the LDAP directory without providing a password, their use is therefore strongly discouraged.


## Bad Practice

```java
// Set up the environment for creating the initial context
Hashtable<String, Object> env = new Hashtable<String, Object>();
env.put(Context.INITIAL_CONTEXT_FACTORY, "com.sun.jndi.ldap.LdapCtxFactory");
env.put(Context.PROVIDER_URL, "ldap://localhost:389/o=JNDITutorial");

// Use anonymous authentication
env.put(Context.SECURITY_AUTHENTICATION, "none"); // Insecure

// Create the initial context
DirContext ctx = new InitialDirContext(env);
```

## Recommended

```java
Hashtable<String, Object> env = new Hashtable<String, Object>();
env.put(Context.INITIAL_CONTEXT_FACTORY, "com.sun.jndi.ldap.LdapCtxFactory");
env.put(Context.PROVIDER_URL, "ldap://localhost:389/o=JNDITutorial");

// Use simple authentication
env.put(Context.SECURITY_AUTHENTICATION, "simple");
env.put(Context.SECURITY_PRINCIPAL, "cn=S. User, ou=NewHires, o=JNDITutorial");
env.put(Context.SECURITY_CREDENTIALS, getLDAPPassword());

// Create the initial context
DirContext ctx = new InitialDirContext(env);
```
Simple authentication alone does not guarantee security however, since LDAP does not also provide encryption or validation. Use LDAP over a secure connection (see [LDAPS](https://ldapwiki.com/wiki/Using%20LDAPS%20With%20JNDI)) for best results.

