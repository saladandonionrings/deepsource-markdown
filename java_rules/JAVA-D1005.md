# Parameter tag has no description
**ID:** `JAVA-D1005` | **Lien:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-D1005)

![Major](https://img.shields.io/badge/severity-major-orange) ![Documentation](https://img.shields.io/badge/type-documentation-lightgrey)

This method/constructor has a parameter tag with no description.

Consider adding a description to the tag. If the parameter's name is clear enough, consider removing the parameter tag entirely.


## Bad Practice
In the example below, there is an `@param` tag for the parameter `someParam` , but it does not attempt to describe the parameter itself. An `@param` tag without a description is no better than omitting the tag entirely.


```java
/**
 * Some method.
 *
 * @param someParam
 */
String someMethod(int someParam) {
    // ...
```

## Recommended
Add a description to the tag.


```java
/**
 * Some method.
 *
 * @param someParam - Does something.
 *
 */
String someMethod(int someParam) {
    // ...
```
