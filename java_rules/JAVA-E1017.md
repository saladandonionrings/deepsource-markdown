# Methods must not unconditionally call themselves
**ID:** `JAVA-E1017` | **Link:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-E1017)

![Critical](https://img.shields.io/badge/severity-critical-red)![Bug Risk](https://img.shields.io/badge/type-bug_risk-green)

This method appears to call itself unconditionally.

Such unconditional recursion can cause stack overflows unless an unexpected exception is thrown.

Recursive methods must always contain an obvious exit condition. This has two advantages:

It may be that the recursive method in question performs an operation that results in an exception being thrown.

Such an exception would effectively cut the recursion short, but when there is no indication to the reader of the code that such an event would occur, confusion is the likely result.

If this was not done intentionally, it would indicate a bug since the execution of such a method would only end once the executing thread suffers a stack overflow.


## Bad Practice
The only way this method could break out of the recursive loop is by throwing an exception.

Perhaps `doSomething()` may throw an exception in some cases?


```java
void recursiveMethod() {
    doSomething();
    recursiveMethod(); // Unconditional!
}
```

## Recommended
Here, when the stop condition is true, we return without further recursion.


```java
void recursiveMethod() {
    if (stopCondition) return
    doSomething();
    recursiveMethod();
}
```
