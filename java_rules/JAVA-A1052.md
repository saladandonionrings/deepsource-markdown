# Audit: `DocumentBuilder` may be vulnerable to XXE attacks
**ID:** `JAVA-A1052` | **Link:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-A1052)

![Critical](https://img.shields.io/badge/severity-critical-red)![Security](https://img.shields.io/badge/type-security-red)

This code appears to use a `DocumentBuilder` instance without setting the correct input processing flags. This could allow [XML External Entity (XXE)](https://en.wikipedia.org/wiki/XML_external_entity_attack) attacks to easily occur.

To put into perspective how XXE attacks can cause damage, consider the following examples:

**Exposing Local File Data**


```java
<?xml version="1.0" encoding="ISO-8859-1"?>
<!DOCTYPE foo [
   <!ENTITY xxe SYSTEM "file:///etc/passwd" > ]>
<foo>&xxe;</foo>
```
The example above uses XML's DTD syntax to define an XML entity whose data is present outside the file itself (it is therefore an Xml eXternal Entity). That entity (`&xxe` here) is then used as the value of an XML element, `<foo>`.

It so happens that the value of the external entity is specified to be the `/etc/passwd` file of the local machine, which is in general private information which must not be shared, leave alone accessed by the server process in any way. If an attacker could upload a malicious XML file with this particular declaration in it, the resulting XML file when parsed will also evaluate the external entity, and by extension, load the contents of `/etc/passwd`.

If the resultant data can be downloaded by the attacker again by some means, we would have described a successful data exfilteration attack.

**XEE Denial of Service**


```java
<?xml version="1.0"?>
<!DOCTYPE lolz [
 <!ENTITY lol "lol">
 <!ELEMENT lolz (#PCDATA)>
 <!ENTITY lol1 "&lol;&lol;&lol;&lol;&lol;&lol;&lol;&lol;&lol;&lol;">
 <!ENTITY lol2 "&lol1;&lol1;&lol1;&lol1;&lol1;&lol1;&lol1;&lol1;&lol1;&lol1;">
 <!ENTITY lol3 "&lol2;&lol2;&lol2;&lol2;&lol2;&lol2;&lol2;&lol2;&lol2;&lol2;">
[...]
 <!ENTITY lol9 "&lol8;&lol8;&lol8;&lol8;&lol8;&lol8;&lol8;&lol8;&lol8;&lol8;">
]>
<lolz>&lol9;</lolz>
```
The above example abuses DTD syntax to create an "XEE bomb". An XML Entity Expansion (XEE) bomb is a type of Denial of Service (DoS) attack that makes use of XML's DTD syntax. It is possible to define a set of XML entities, each of which expand into others, to use up exponential amounts of CPU time and memory which would in turn bring the application to a grinding halt.

This particular attack works because the `lol9` entity defined in the DTD tag recursively expands into an exponentially increasing set of other entities as defined, until the expansion terminates, resulting in ~10^9 instances of the `lol` entity being created. It is likely that this will trigger an Out of Memory (OOM) crash in the best case, or possibly may render the application process completely unresponsive.


## Bad Practice
In the example below, a `DocumentBuilder` has been created, which neither prevents processing of DTD elements (which would completely stop any XXE or XEE attacks) nor prevents access to external files via secure processing.


```java
// No flags set.
DocumentBuilder builder = DocumentBuilderFactory.newInstance().newDocumentBuilder();
```

## Recommended
Here are two ways XXE attacks can be avoided.

* Using the[**`XMLConstants.FEATURE\_SECURE\_PROCESSING`
**](https://docs.oracle.com/javase/8/docs/api/javax/xml/XMLConstants.html#FEATURE_SECURE_PROCESSING)flag
Enabling this flag will not disallow DTD processing, but will prevent DoS attacks by limiting recursion, and will prevent access to external resources.


```java
DocumentBuilderFactory dbf = DocumentBuilderFactory.newInstance();
  dbf.setFeature("http://javax.xml.XMLConstants/feature/secure-processing", true);
  DocumentBuilder db = dbf.newDocumentBuilder();
```
* Disallowing DTD processing entirely
We can disable DTD processing wholesale by directly specifying the `disallow-doctype-decl` feature flag:


```java
DocumentBuilderFactory dbf = DocumentBuilderFactory.newInstance();
dbf.setFeature("http://apache.org/xml/features/disallow-doctype-decl", true);
DocumentBuilder db = dbf.newDocumentBuilder();
```
This will prevent DTD entities from being processed. If you are reliant on certain DTD related features, consider using the secure processing flag instead.

