# Case insensitive regex does not properly handle Unicode input
**ID:** `JAVA-E1010` | **Link:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-E1010)

![Major](https://img.shields.io/badge/severity-major-orange)![Bug Risk](https://img.shields.io/badge/type-bug_risk-green)

Java's regex implementation can be configured to be case insensitive, but unless further steps are taken, such case insensitive regular expressions may not be able to handle Unicode input correctly.

This can lead to instances where upper and lower case Unicode characters will be incorrectly recognized as different characters (Ideally, they should be treated as the same character).

Java provides the `Pattern.CASE_INSENSITIVE` flag, as well as the `(?i)` regex group construct to enable case insensitive mode in regular expressions. However, these features only account for ASCII letters; any Unicode characters (like `Ä` and `ä`) will not be processed properly and will be treated as separate characters.

To remedy this, Java provides the `Pattern.UNICODE_CASE` and `Pattern.UNICODE_CHARACTER_CLASS` flags which can be used in tandem with the `CASE_INSENSITIVE` flag to extend the case insensitive behavior to non-ASCII characters. The `(?u)` and `(?U)` regex group constructs can also be used in place of these flags to achieve the same purpose.


## Bad Practice

```java
input = "Смотри́";

// The regex tries to match the string "смотри́" case insensitively.
Pattern withFlag = Pattern.compile("смотри́", Pattern.CASE_INSENSITIVE);
Pattern withGroup = Pattern.compile("(?i)смотри́");

boolean matches = withFlag.matcher(input).matches(); // FALSE!
matches = withGroup.matcher(input).matches(); // FALSE!
```

## Recommended
Use the `UNICODE_CASE`/`UNICODE_CHARACTER_CLASS` flags or the `?u`/`?U` group flags to allow Unicode pattern matching.


```java
Pattern withGoodGroup = Pattern.compile("(?iu)смотри́");
Pattern withGoodFlag = Pattern.compile("смотри́", Pattern.CASE_INSENSITIVE | Pattern.UNICODE_CASE);

matches = withGoodFlag.matcher(input).matches(); // true.
matches = withGoodGroup.matcher(input).matches(); // true.
```
**Note:** if you only want to enable case insensitive matching for Unicode, it may be better to use only the `UNICODE_CASE` flag, or the corresponding group flag, `?u` instead of the `UNICODE_CHARACTER_CLASS` flag or the `?U` group flag. This is because the `UNICODE_CHARACTER_CLASS` flag also enables certain [Unicode-related features](https://docs.oracle.com/javase/8/docs/api/java/util/regex/Pattern.html#UNICODE_CHARACTER_CLASS) that may impact performance.

