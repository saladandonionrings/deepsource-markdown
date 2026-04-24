# Case conversion may not work as expected for international characters without specifying the encoding
**ID:** `JAVA-S0069` | **Lien:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-S0069)

![Minor](https://img.shields.io/badge/severity-minor-yellow) ![Bug Risk](https://img.shields.io/badge/type-bug_risk-green)

A String is being converted to upper or lowercase, using the platform's default encoding. This may result in improper conversions when used with international characters.

Use `String.toUpperCase( Locale l )` / `String.toLowerCase( Locale l )` versions instead to avoid surprises.


## Examples

## Problematic Code
Consider the case when platform default locale is Turkish:


```java
Locale.setDefault(new Locale("tr", "TR"));

String a = "TITLE".toLowerCase();

assert(a == "title"); // Fails.
assert(a == "tıtle"); // Succeeds. Notice the missing dot on the "ı".
```

## Recommended
We can resolve this mismatch by specifying the locale while performing the conversion:


```java
String a = "TITLE".toLowerCase(Locale.ENGLISH); 

assert(a == "title"); // Succeeds as expected.
```
