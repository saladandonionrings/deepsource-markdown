# Undocumented constructor found
**ID:** `JAVA-D1002` | **Link:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-D1002)

![Minor](https://img.shields.io/badge/severity-minor-yellow)![Documentation](https://img.shields.io/badge/type-documentation-lightgrey)

This constructor does not have any documentation.

Consider adding a documentation comment to explain its use.

While it may seem like the usage of a constructor is perfectly obvious, any consumers of your API may not be able to pick up on certain details.


## Bad Practice
In the example below, it is difficult to understand what the arguments of this constructor for `DataForwarder` signify.


```java
class DataForwarder {
    public DataForwarder(int nTypes, String... values) {
        // ...
    }
}
```

## Recommended
Document the constructor and provide useful information about it or its parameters as required.


```java
/**
 * Initializes a DataForwarder with a set of types to filter by, along with their names.
 *
 * @param nTypes - The number of types to filter by.
 * @param values - The input type names to filter by.
 */
public DataForwarder(int nTypes, String... values) {
    // ...
}
```

## Exceptions
If you feel this constructor does not require documentation, you can add a skipcq comment to this constructor.

