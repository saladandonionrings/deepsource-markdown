# Malformed JavaDoc comment
**ID:** `JAVA-D1007` | **Link:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-D1007)

![Major](https://img.shields.io/badge/severity-major-orange)![Documentation](https://img.shields.io/badge/type-documentation-lightgrey)

This JavaDoc comment appears to be malformed. Such comments may raise errors or cause crashes when passed to a JavaDoc generation tool.

Rewrite the comment to use correct syntax.

JavaDoc is generally quite permissive with respect to what counts as a valid tag name. However, it is important to avoid straying too much from established conventions to ensure that your documentation does not leave people scratching their heads.


## Bad Practice
In the example below, there is a malformed `@param` tag starting with two `@` symbols. If a JavaDoc tool were to be used on this code, it is likely that errors will be raised.


```java
/**
 * some method.
 *
 * @@param someParam param.
 */
void someMethod(String someParam) {
    // ...
}
```

## Recommended
Correct the improper syntax, and follow proper JavaDoc comment formatting.


```java
/**
 * some method.
 *
 * @param someParam param.
 */
void someMethod(String someParam) {
    // ...
}
```
