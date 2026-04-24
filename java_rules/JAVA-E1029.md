# Whitespace characters must always be escaped
**ID:** `JAVA-E1029` | **Link:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-E1029)

![Critical](https://img.shields.io/badge/severity-critical-red)![Bug Risk](https://img.shields.io/badge/type-bug_risk-green)

There are many whitespace characters other than the space character (`' '`) defined in the Unicode standard. However, using these characters without properly escaping them can cause unintended behavior, bugs or even a security breach to occur.

There has been an example of a security vulnerability due to the lack of escaping certain whitespace characters: [CVE-2021-42574](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2021-42574).


## Bad Practice
This issue is raised when any whitespace character other than `' '` (the space character) is used without an escape sequence.


```java
String withATab = "A    B";
String withZeroWidthSpace = "abc​def"; // There's a character between abc and def here.
char tabChar = '    ';
System.out.println("5678,‮6776, 4321‬, USD");
```
Try selecting the text of the last line in this example; you may notice some strange behavior...

This is due to the use of the Unicode right-to-left override character ([`U+202E`](https://www.unicode.org/reports/tr9/#Explicit_Directional_Overrides)), which lets us force the following characters to be formatted as right-to-left, and the pop directional formatting character ([`U+202C`](https://www.unicode.org/reports/tr9/#Terminating_Explicit_Directional_Embeddings_and_Overrides)) which removes the current directional formatting override.


## Recommended
Always escape whitespace characters which are not spaces.


```java
char goodTab = '    ';
String goodStringWithTab = "A   B";
String withZeroWidthSpace = "abc​def";
String bidiText = "5678, ‮6776, 4321‬, USD"
```
