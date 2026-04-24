# Undocumented declaration found
**ID:** `JAVA-D1003` | **Lien:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-D1003)

![Minor](https://img.shields.io/badge/severity-minor-yellow) ![Documentation](https://img.shields.io/badge/type-documentation-lightgrey)

This declaration is not documented.

Consider adding a documentation comment to explain its functionality.

While it may seem like the functionality of this declaration is perfectly obvious, any consumers of your API or future maintainers may not be able to pick up on certain details.


## Bad Practice
In the example below, the meaning of `AUIHighlight` may not be entirely clear, and questions such as what `AUI` means may pop up.


```java
public enum AUIHighlight {
    LIGHT_BLUE(0x00ADD8E6),
    DARK_BLUE(0x0000008B),
    // ...

    private int value;
    AUIHighlight(int val) {
        value = val;
    }
}
```

## Recommended
Make sure to document any non-obvious details about any code element.


```java
/**
 * UI highlight color values for the action bar of the application.
 */
public enum AUIHighlight {
    // ...
}
```
