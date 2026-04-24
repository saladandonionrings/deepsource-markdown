# Switch case without appropriate control flow break
**ID:** `JAVA-A1068` | **Lien:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-A1068)

![Major](https://img.shields.io/badge/severity-major-orange) ![Bug Risk](https://img.shields.io/badge/type-bug_risk-green)

This switch case does not terminate in a `break` or other such control flow statement. This can result in accidental switch case fallthrough, unintentionally executing code.


## Bad Practice

```java
switch (something) {
    case 1: {
        // Do something...
        // No break statement...
    case 2:
        // do something else...
        break;
    default:
        // Default behavior.
}
```

## Recommended
Add a break statement at the end of each case unless the fallthrough behavior is intentional.


```java
switch (something) {
    case 1: {
        // Do something...
        break;
    case 2:
        // do something else...
        break;
    default:
        // Default behavior.
}
```

## Exceptions
This issue can be safely ignored if the fallthrough behavior was intentional.

