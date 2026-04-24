# Overwriting a method parameter will not modify the original object
**ID:** `JAVA-S0352` | **Link:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-S0352)

![Critical](https://img.shields.io/badge/severity-critical-red)![Bug Risk](https://img.shields.io/badge/type-bug_risk-green)

This method ignores the original value of a parameter and attempts to assign a new value to it.

This often indicates a mistaken belief that the write to the parameter will be conveyed back to the caller. Because a parameter is just a copy of a reference from the calling scope, overwriting it will only modify the method's local copy of the reference, not the calling scope's copy.

However, note that it is still possible to modify a value passed to a method if its public interface permits it.

Do not assign a new value to parameter references, it will not affect the original value.


## Example

```java
void method(Float param) {

    param = 3.2f; // Will not affect value of param in the calling scope.

    // ...
}


void method1(HashMap<String, Integer> param) {
    // ...

    param.put("abc", 3); // Modifies the object pointed to by param instead of the reference itself.

    // ...
}
```
