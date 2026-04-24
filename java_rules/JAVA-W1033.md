# Imprecise redefinition of library constant
**ID:** `JAVA-W1033` | **Link:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-W1033)

![Major](https://img.shields.io/badge/severity-major-orange)![Anti-pattern](https://img.shields.io/badge/type-anti_pattern-purple)

A library constant has been redefined in source code with a different/imprecise value.

It's recommended to use the predefined library constant for code clarity and better precision.


## Bad Practice

```java
static final double PI_bad = 3.14;

double bad = 2 * PI_bad * radius;
```

## Recommended

```java
double good = 2 * Math.PI * radius;
```
