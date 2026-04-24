# Concrete collection type used in method declaration
**ID:** `JAVA-W1065` | **Link:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-W1065)

![Major](https://img.shields.io/badge/severity-major-orange)![Anti-pattern](https://img.shields.io/badge/type-anti_pattern-purple)

Concrete collection types (such as `ArrayList`, `HashMap`, etc.) should not be used in a `public` method's signature.

Java encourages the use of abstract types/interfaces at the API boundary over concrete types. This helps one design generic APIs that are easy to modify and extend.

Although designing generic APIs is generally preferable, one should especially emphasize their use over concrete types when elements of the collection API are involved. This is because almost all non-trivial Java applications depend heavily on abstract types defined in Java's collection framework.


## Bad Practice

```java
// Return type is `ArrayList` instead of `List`.
public ArrayList<Integer> method() {
    // ..rest of the code
}

// Parameter type is `HashMap` instead of `Map`.
public void methodWithParams(HashMap<String, String> demo) {
    // ..rest of the code
}
```

## Recommended
Consider using abstract types in return values and parameters of `public` methods.


```java
public List<Integer> method() {
    // ..rest of the code
}

public void methodWithParams(Map<String, String> demo) {
    // ..rest of the code
}
```
