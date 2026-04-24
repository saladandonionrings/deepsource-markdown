# Unmatched Parameter tag found
**ID:** `JAVA-D1004` | **Link:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-D1004)

![Major](https://img.shields.io/badge/severity-major-orange)![Documentation](https://img.shields.io/badge/type-documentation-lightgrey)

This method/constructor's parameter tags don't match its declared parameters.

This may confuse people who read the method's documentation. Remove/replace the misplaced tags so that each parameter has a matching JavaDoc description.


## Bad Practice
In the example below, `someMethod` does not have any parameter named `nonexistant`, but its Javadoc states that such a parameter exists. This may have happened due to a refactor that changed a parameter's name, or, as in this case, removed an existing parameter.


```java
/**
 * Some method.
 *
 * @param someParam - Does something.
 * @param nonexistant - Doesn't exist.
 * @return the result of this operation.
 *
 */
String someMethod(int someParam) {
    // ...
```

## Recommended
Make sure to only document parameters that actually exist.


```java
/**
 * Some method.
 *
 * @param someParam - Does something.
 * @return the result of this operation.
 *
 */
String someMethod(int someParam) {
    // ...
```
