# Methods must not be empty
**ID:** `JAVA-W1004` | **Link:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-W1004)

![Minor](https://img.shields.io/badge/severity-minor-yellow)![Anti-pattern](https://img.shields.io/badge/type-anti_pattern-purple)

This method is empty and does not appear to have any explanation regarding why. This may confuse future readers of this code. Additionally, such code produces needless, avoidable clutter.


## Bad Practice

```java
void method() {

}
```

## Recommended
If the method overrides a parent implementation and is left empty to prevent default behavior, consider documenting why with a comment:


```java
@Override
void method() {
    // left empty because of ...
}
```
