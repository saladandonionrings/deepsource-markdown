# Possible null access
**ID:** `JAVA-E1083` | **Link:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-E1083)

![Critical](https://img.shields.io/badge/severity-critical-red)![Bug Risk](https://img.shields.io/badge/type-bug_risk-green)

This code contains a possible null pointer dereference. Double-check the code to ensure that the concerned variable always has a non-null value when accessed.


## Bad Practice
In the example below, if `someCondition` is true, it is possible for `value` to be null when it reaches the assignment of `valLen`.


```java
String value = "something";
if (someCondition) {
    value = null;
}

// ...

int valLen = value.length;  // Null pointer exception!
```

## Recommended
Check for null before you use the value at the vulnerable point in code.


```java
int valLen = value != null ? value.length : 0;
```
If `value` was instead never intended to be null, consider changing the preceding logic to ensure that `value` is at least set to a safe default wherever possible:


```java
if (someCondition) {
    value = "";
}

int valLen = value.length;  // No exception thrown.
```
