# `StringBuffer`/`StringBuilder` constructors should not be passed characters as the first argument
**ID:** `JAVA-E1023` | **Link:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-E1023)

![Critical](https://img.shields.io/badge/severity-critical-red)![Bug Risk](https://img.shields.io/badge/type-bug_risk-green)

`StringBuffer`/`StringBuilder` constructors should not be passed character values, as this will not yield the expected result (an initialized object with an initial single character string). It will instead create an empty `StringBuffer`/`StringBuilder` with a capacity value set to the ASCII value of the character passed as an argument.

`StringBuffer` and `StringBuilder` both have a [constructor](https://docs.oracle.com/javase/8/docs/api/java/lang/StringBuilder.html#StringBuilder-int-) that allows one to set the initial capacity of the contained string. They also have a [constructor](https://docs.oracle.com/javase/8/docs/api/java/lang/StringBuilder.html#StringBuilder-java.lang.String-) which can be used to set the initial string to build from.

Due to Java's [numeric type promotion rules](https://docs.oracle.com/javase/specs/jls/se7/html/jls-5.html#jls-5.1.2), `char` values will automatically be promoted to a numeric type such as `int` if a `char` is passed where a number was expected.

For example:

Passing `'a'` (ASCII code 97) to a method expecting an `int` value will first convert `'a'` to the `int` value 97 before passing it to the method.

Consider this code now:


```java
StringBuffer sb = new StringBuffer('a');
```
While the expectation may be that `sb` will be initialized with a single character string (`"a"`), `sb` will instead be initialized as an empty `StringBuffer` with an initial capacity of 97 characters.

It is possible that the developer may accidentally pass a character value (like `'a'` for example) instead of passing a string (like `"a"`) to the constructor of either of `StringBuffer`/`StringBuilder`. This may be because of the assumption that `StringBuffer` or `StringBuilder` can accept a single character as an argument, or because of a mistake when typing quotes for a single character string.

If this was intentional, it makes for very abstruse code and is better off replaced.


## Bad Practice

```java
// Equivalent to creating a StringBuilder with an initial capacity of 97 characters.
StringBuilder sb = new StringBuilder('a');
```

## Recommended

```java
StringBuilder sb = new StringBuilder("a");
```
If you wish to create the `StringBuilder` with a specific initial capacity, Just specify it directly:


```java
StringBuilder sb = new StringBuilder(100);
```
