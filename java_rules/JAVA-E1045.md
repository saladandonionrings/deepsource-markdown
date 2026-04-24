# Loop conditions should be true at least once
**ID:** `JAVA-E1045` | **Lien:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-E1045)

![Major](https://img.shields.io/badge/severity-major-orange) ![Bug Risk](https://img.shields.io/badge/type-bug_risk-green)

This loop's condition is never true, meaning it will never execute (or, for a `do` loop, only execute once). Check the condition and rectify it if there are mistakes. If used to disable the code, consider commenting the code out, or removing the loop entirely.


## Bad Practice

```java
while (false) {
    // Will not execute...
}

// ...

int a = 2;

// this loop will never start...
while (a < 0) {
    // ...
    a += 2;
}
```

## Recommended
Remove the code or fix the loop.


```java
int a = 2;

// The loop condition is now fixed.
while (a < 20) {
    // ...
    a += 2;
}
```
