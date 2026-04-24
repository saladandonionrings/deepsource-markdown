# JavaDoc tags should not be empty
**ID:** `JAVA-D1006` | **Lien:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-D1006)

![Major](https://img.shields.io/badge/severity-major-orange) ![Documentation](https://img.shields.io/badge/type-documentation-lightgrey)

Empty JavaDoc tags are meaningless, and may even cause tools that consume JavaDoc comments to crash.

Always add an explanation when writing JavaDoc tags.


## Bad Practice
In the example below, the `@param` JavaDoc tag does not refer to any parameter, and does not have any description. This may be because the documentation comment itself is incomplete.


```java
/**
 * Some method.
 *
 * @param
 */
String someMethod(int someParam) {
    // ...
```

## Recommended
Avoid leaving JavaDoc tags without any associated info or description.


```java
/**
 * Some method.
 *
 * @param someParam - Specifies something.
 */
String someMethod(int someParam) {
    // ...
```
