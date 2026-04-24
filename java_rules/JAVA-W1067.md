# Redundant cast of return value
**ID:** `JAVA-W1067` | **Link:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-W1067)

![Major](https://img.shields.io/badge/severity-major-orange)![Anti-pattern](https://img.shields.io/badge/type-anti_pattern-purple)

The return value of a method call should not be cast if it is used in an expression that expects a value of the same type that the method is returning.

Casting the return value of a method is generally considered bad practice because it can lead to unnecessary complexity and makes the code harder to read.

Unnecessary casting can also be confusing if the method's return type is either the same as the type of the cast or if the return type inherits from the cast type.


## Bad Practice

```java
public List<String> manyStrings() {
    return List.of("a", "b");
}

for (String s : (List<String>) manyStrings()) { // ... }
```

```java
public String oneString() {
    return "ab";
}

Object value = oneString();
```

## Recommended
Consider removing the redundant casts.


```java
public List<String> manyStrings() {
    return List.of("a", "b");
}

for (String s : manyStrings()) { // ... }
```

```java
public String oneString() {
    return "ab";
}

Object value = oneString();
```
