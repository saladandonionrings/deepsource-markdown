# Undocumented method found
**ID:** `JAVA-D1001` | **Link:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-D1001)

![Minor](https://img.shields.io/badge/severity-minor-yellow)![Documentation](https://img.shields.io/badge/type-documentation-lightgrey)

This method does not have any documentation.

Consider adding a documentation comment to explain its use.

While it may seem like the usage of a method is perfectly obvious, any consumers of your API may not be able to pick up on certain details.


## Bad Practice
This method appears to return an address string based on its name and the return type, but it is hard to say anything further about it. We neither know if the address will be formatted in a special way nor are we given any useful information regarding the returned value.


```java
String getAddress() {
    // ...
}
```

## Recommended
Provide a detailed description of what the method does.


```java
/**
 * Returns the address in 3 line format,
 * with street address on line 1, area/city on line 2
 * and state and country as well as postal code on line 3.
 */
String getAddress() {
    // ...
}
```
