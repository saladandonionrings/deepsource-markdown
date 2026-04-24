# Multiple variables declared on the same line
**ID:** `JAVA-C1003` | **Link:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-C1003)

![Major](https://img.shields.io/badge/severity-major-orange)![Style](https://img.shields.io/badge/type-style-blue)

Multiple variables (or fields) should not be declared on the same line.

Declaring more than one variables (or fields) on the same line makes the code harder to read. Things might get more confusing if some of those variables are initiliazed and some of them are not.


## Bad Practice

```java
class Klass {
    private int a, b = 20;

    private void method() {
        double d1, d2 = 3.5, d3;
        // ... rest of the code
    }
}
```

## Recommended
Consider declaring one variable per line.


```java
class Klass {
    private int a;
    private int b = 20;

    private void method() {
        double d1;
        double d2 = 3.5;
        double d3;
        // ... rest of the code
    }
}
```
