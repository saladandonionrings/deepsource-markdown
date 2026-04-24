# Collection and array length checks must be sensible
**ID:** `JAVA-W1005` | **Lien:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-W1005)

![Major](https://img.shields.io/badge/severity-major-orange) ![Anti-pattern](https://img.shields.io/badge/type-anti_pattern-purple)

A comparison involving the size of a collection or an array was found, which is always guaranteed to be either true or false.

This is a generally frivolous action and may have been caused by a typo. If this was done to prevent certain code from executing or to ensure some code always executes for debugging purposes, remove it. All debugging-related code changes should be cleaned up after their purpose is served.

Consider revisiting the expression to make sure such a comparison was intended.


## Bad Practice
Certain comparisons don't ever make sense when dealing with the size of a collection or an array's length.

Consider:


```java
someArray.length < 0
```
The expression above always evaluates to `false` since it is impossible for an array to have a negative length.

Similarly, the following comparison is functionally useless:


```java
someCollection.size() >= 0
```
The usual state of affairs is that collections cannot have negative sizes. It thus makes no sense to explicitly check if the size of a collection is non-negative as has been done here.


## Recommended
Do not perform comparisons that are guaranteed to result in the same value everytime.


```java
if (coll.size() > 24) {
    // ...
}
```
If you need to check for when a collection is empty, you can use the `isEmpty()` method to do so:


```java
if (coll.isEmpty()) {
    // ...
}
```
