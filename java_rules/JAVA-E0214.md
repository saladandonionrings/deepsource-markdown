# For loop appears to check one variable and increment another
**ID:** `JAVA-E0214` | **Lien:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-E0214)

![Major](https://img.shields.io/badge/severity-major-orange) ![Bug Risk](https://img.shields.io/badge/type-bug_risk-green)

There is a complicated, subtle or wrong increment in this `for` loop. It appears that the variable being checked in the loop's condition is not the same as the one being updated.

This issue is usually caused by a typo. Always be mindful of the loop variable being checked or updated, especially in nested loops.


## Bad Practice

```java
for (int i = 0; i < 20; i++) {
    for (int j = i; j < 20; i++) { // i is updated, not j.
        // ...
    }
}
```
In most cases, this will result in an infinite loop.


## Recommended
Ensure that the variable which is checked in the condition is what is also updated.


```java
for (int i = 0; i < 20; i++) {
    for (int j = i; j < 20; j++) { // j is updated now, as it should be.
        // ...
    }
}
```

## Exceptions
Sometimes, the variable may be updated in a more non-obvious way. In such cases, it is safe to ignore this issue as long as you can verify that the variable is truly updated properly.

If this is intended, make sure to document the behavior if what is going on isn't easily obvious.

