# Redundant boolean literal
**ID:** `JAVA-W1064` | **Lien:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-W1064)

![Major](https://img.shields.io/badge/severity-major-orange) ![Bug Risk](https://img.shields.io/badge/type-bug_risk-green)

Boolean literals should not be used redundantly within expressions.

An entity that may evaluate to true or false can directly be used in an expression where a boolean value is expected. Boolean literals are almost never necessary in any expression.


## Bad Practice

```java
public void method() {
    if (returnsBoolean() == true) { //.. }
    if (boolVar || false) { // .. }
    if (boolVar && true) { // .. }
}
```

## Recommended
Consider removing the redundant literals.


```java
public void method() {
    if (returnsBoolean()) { //.. }
    if (boolVar) { // .. }
    if (boolVar) { // .. }
}
```
