# Disabling escaping of special characters in templates is a security risk
**ID:** `JAVA-S1025` | **Link:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-S1025)

![Critical](https://img.shields.io/badge/severity-critical-red)![Security](https://img.shields.io/badge/type-security-red)

Automatic variable escaping should not be disabled when using template processing systems such as Mustache or FreeMarker.

When special characters (such as '<', '>', or '/') are encountered, templating engines such as JMustache can automatically replace them with equivalent escape sequences. This prevents malicious inputs from inserting executable code into the final page, leading to an XSS (Cross Site Scripting) attack.

However, character escaping is context sensitive and characters that are safe to keep unescaped outside of HTML tags may not be safe to leave alone within attributes, or vice versa. In the example below, a template engine that does not escape `':'` characters has been used.


```java
<a href="{{ myLink }}">link</a>
```
If `myLink`'s value were set to a JavaScript scheme string such as `javascript:alert('hack')`, the resultant HTML could become the setup for an XSS attack:


```java
<a href="javascript:alert('hack')">link</a>
```
This issue is reported when HTML escaping is disabled in the [JMoustache](https://github.com/samskivert/jmustache) and [FreeMarker](https://freemarker.apache.org/) template engines.


## Bad Practice
When using JMoustache, do not call `escapeHTML()` with a `false` value, or set the escaper to [`Escapers.NONE`](http://samskivert.github.io/jmustache/apidocs/com/samskivert/mustache/Escapers.html).


```java
Mustache
    .compiler()
    .escapeHTML(false)          // Not good.
    .withEscaper(Escapers.NONE) // Not good either.
    .compile(template)
    .execute(context);
```
When using FreeMarker, do not call [`Configuration.setAutoEscapingPolicy()`](https://freemarker.apache.org/docs/api/freemarker/template/Configuration.html#setAutoEscapingPolicy-int-) with `DISABLE_AUTO_ESCAPING_POLICY`.


```java
Configuration config = new Configuration();
config.setAutoEscapingPolicy(Configuration.DISABLE_AUTO_ESCAPING_POLICY);
```

## Recommended
In JMustache, auto-escaping is turned on by default; there is no need to explicitly set the behavior, but you could do so by passing `true` to `Compiler.escapeHTML()`, or by passing `Escapers.HTML` to `Compiler.withEscaper()`.


```java
Mustache
    .compiler()                 // Doing nothing is an option.
    .escapeHTML(true)           // But you can set escapeHTML to true if you want to.
    .withEscaper(Escapers.HTML) // Or set the escaper to Escapers.HTML.
    .compile(template)
    .execute(context);
```
FreeMarker's auto-escaping is also turned on by default for HTML; but you can force it to be on by calling `setAutoEscapingPolicy()` with the right arguments:


```java
config.setAutoEscapingPolicy(Configuration.ENABLE_IF_DEFAULT_AUTO_ESCAPING_POLICY); // This is the default.
config.setAutoEscapingPolicy(Configuration.ENABLE_IF_SUPPORTED_AUTO_ESCAPING_POLICY); // This is also a viable option.
```
