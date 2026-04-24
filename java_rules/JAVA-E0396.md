# Increment/decrement performed during assignment expression to same variable will be lost
**ID:** `JAVA-E0396` | **Lien:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-E0396)

![Major](https://img.shields.io/badge/severity-major-orange) ![Bug Risk](https://img.shields.io/badge/type-bug_risk-green)

The code performs a post increment/decrement operation (e.g., `i++` ) and then immediately overwrites it.

Either change the assignment into an operator assignment or write the increment as a separate statement.


## Bad Practice

```java
i = 0;

i = i++;

assert(i == 1); // Fails, i == 0

i = 0
```

## Recommended

```java
i += 1;

assert(i == 1); // succeeds

i++;

assert(i == 2; // Also succeeds.
```
The add-assign operator ( `+=` ) is very useful for cases where one wishes to increment a value; alternatively, just a lone increment expression could serve the same purpose.

