# Possible null access due to exception handling
**ID:** `JAVA-E1084` | **Lien:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-E1084)

![Critical](https://img.shields.io/badge/severity-critical-red) ![Bug Risk](https://img.shields.io/badge/type-bug_risk-green)

This code contains a possible null dereference that may occur based on whether an exception is thrown or not. Carefully check your code to ensure that the concerned value can never be null at this point.


## Bad Practice
In the snippet below, whether the value of `s` stays `null` depends on whether `mayThrow()` throws an exception. If an exception were thrown, the code would skip the assignment to `s` , which means it would remain as `null` until it is accessed for a method call. This will lead to a `NullPointerException` being thrown.


```java
String s = null;

try {
    mayThrow();

    s = "SomeValue";
} catch (SomeException e) {
    // no assignment of s
}

int len = s.length; // This will be null if the method call throws!
```

## Recommended
This could be fixed either by performing a check at the usage site, if the logic requires that `s` should possibly be null:


```java
int len = (s == null) ? 0 : s.length();
```
It could also be fixed by changing the preceding logic to correctly set the value of `s` at all points. For example, the catch block could set `s` to a safe default value:


```java
try {
    // ...
} catch (SomeException e) {
    s = "";
}

int len = s.length; // no exception!
```
