# Missing enum elements in switch cases
**ID:** `JAVA-E1082` | **Lien:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-E1082)

![Major](https://img.shields.io/badge/severity-major-orange) ![Bug Risk](https://img.shields.io/badge/type-bug_risk-green)

Switch statements that have expression of an enum type and don't have the default label must specify all the enum elements in their cases.

Even if the current state of the application doesn't require certain enum elements to be considered in switch cases, a patch in the future may change that. When this happens, and the programmer forgets to handle the additional enum elements, this will almost always result in a bug in the application. For this reason, it's highly encouraged to cover all the enum fields in cases of a switch statement.


## Bad Practice

```java
enum Color {
    RED,
    BLUE
}

public void test() {
    Color color = getColor();
    // Bad, missing `Color::BLUE` or `default`.
    switch (color) {
        case RED:
            paintRed();
            break;
    }
}
```
Consider specifying all the enum elements or specify a `default` case.


## Recommended

```java
enum Color {
    RED,
    BLUE
}

public void test() {
    Color color = getColor();
    switch (color) {
        case RED:
            paintRed();
            break;

        case BLUE:
            paintBlue();
            break;
    }
}
```
