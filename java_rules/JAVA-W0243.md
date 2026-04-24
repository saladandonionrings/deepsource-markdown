# Possibly useful method return value is ignored
**ID:** `JAVA-W0243` | **Link:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-W0243)

![Major](https://img.shields.io/badge/severity-major-orange)![Anti-pattern](https://img.shields.io/badge/type-anti_pattern-purple)

This code calls a method and ignores the return value.

The return value is the same type as the type the method is invoked on, and from our analysis it looks like the return value might be important. It may not be a good idea to ignore such return values, since they may have some meaningful value.


## Bad Practice
The following expression's return value would usually not be ignored:


```java
String.toLowerCase()
```
`String` values usually are immutable and all methods defined on `String` return a new value instead of modifying an existing one. Ignoring the return value of such methods will usually result in logic errors or erroneous output.


## Recommended
Always check the return value of any method that does not return void.


```java
String lower = String.toLowerCase()
```

## Exceptions
If it is known that the method has side effects and the return value is not required to be checked for your purposes, it may be perfectly valid to ignore the return value. Always be careful of such logic and make sure to properly document the reason for such calls. This will help future readers understand the intent behind the code.

