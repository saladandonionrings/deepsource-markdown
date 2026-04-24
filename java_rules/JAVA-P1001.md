# Use `String.replace()` instead of `String.replaceAll()` for simple text patterns
**ID:** `JAVA-P1001` | **Link:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-P1001)

![Major](https://img.shields.io/badge/severity-major-orange)![Performance](https://img.shields.io/badge/type-performance-white)

`String.replaceAll()` is a method that accepts a regex string, and replaces all occurrences of the regex with the provided replacement string.

If you are only trying to replace a specific substring and not a more general pattern, it will be better to use `String.replace()` instead.

`String.replaceAll()` internally compiles the given regex string and then replaces all instances of the resulting `Pattern` with the replacement string. Using `replaceAll()` with a pattern string that contains no special characters (e.g. "abc", or "good bye") will cause a regex to be compiled unnecessarily. The same result could be achieved by just performing simpler string comparisons using `String.replace()` instead.

Only use `String.replaceAll()` if you require complex regex pattern matching.


## Bad Practice

```java
String s = "Tha quick brown fox jumped over tha lazy dog.";

s.replaceAll("tha", "the"); // Unnecessary regex compilation
```

## Recommended
`String.replace()` does not compile a regex when replacing instances of the given substring.


```java
s.replace("tha", "the");
```

## Exceptions
If you are indeed trying to match a specific regex based pattern, feel free to disregard this occurrence.

