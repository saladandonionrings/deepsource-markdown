# Audit: Broadcasting intents without specifying a target package or receiver permission may be a security risk
**ID:** `JAVA-A1023` | **Link:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-A1023)

![Critical](https://img.shields.io/badge/severity-critical-red)![Security](https://img.shields.io/badge/type-security-red)

Intents that contain sensitive information should only be broadcast as explicit intents with tight control on what activities may receive them.

Intents in Android can be of two types, implicit or explicit.

It is quite safe to add sensitive information within an explicit intent, since it is possible to be precise about what activity (or activities) are permitted to receive it. However, implicit intents have no such protection, and any activity from any application on one's device will be able to register a broadcast receiver for one. This means that if you transmit security sensitive data (such as an API token) through an implicit intent, it is possible for a malicious application to register a receiver for that intent and access private information.

This issue is raised when the analyzer detects cases of an intent being broadcast implicitly.


## Bad Practice

```java
Intent withSecurityInfo = ...;

context.sendBroadcast(withSecurityInfo); // !!!

// This overload of sendBroadcast allows one to specify what permissions
// an application must have to be allowed to receive this intent.
// Setting the second parameter to null indicates that there are no restrictions!
context.sendBroadcast(withSecurityInfo, null);
```

## Recommended
* **Use explicit intents**

```java
Intent explicit = new Intent(this, SomeComponent.class); // We now provide a specific class that is the intended recipient.

context.sendBroadcast(explicit);
```
* **Use intents with package filters**

```java
Intent withPkgFilter = new Intent(...);

// Now, this intent can only be received by activities in the specified package.
withPkgFilter.setPackage("some.package");
```
