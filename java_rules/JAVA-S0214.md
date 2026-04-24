# For loop appears to check one variable and increment another
**ID:** `JAVA-S0214` | **Lien:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-S0214)

![Major](https://img.shields.io/badge/severity-major-orange) ![Bug Risk](https://img.shields.io/badge/type-bug_risk-green)

There is a complicated, subtle or wrong increment in this for loop. Are you sure this for loop is incrementing the correct variable? It appears that another variable is being initialized and checked by the for loop.

This issue is usually caused by a typo.


## Examples

## Problematic Code

```java
for (int i = 0; i < 20; i++) {
    for (int j = i; j < 20; i++) { // i is updated, not j.
        // ...
    }
}
```
In most cases, this will result in an infinite loop. Always be mindful of the loop variable being checked or updated, especially in nested loops.


```java
for (int i = 0; i < 20; i++) {
    for (int j = i; j < 20; j++) {
        // ...
    }
}
```
If this is intended, make sure to document the behavior if what is going on isn't easily obvious.

