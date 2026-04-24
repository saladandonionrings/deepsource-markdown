# Object should not be passed where a generic type is expected
**ID:** `JAVA-W1036` | **Link:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-W1036)

![Major](https://img.shields.io/badge/severity-major-orange)![Bug Risk](https://img.shields.io/badge/type-bug_risk-green)

This code appears to pass a value of type `java.lang.Object` where a generic typed argument would have been expected. This will cause errors such as `ClassCastException`s if the value passed is not of the correct type at runtime.

Avoid passing `Object` values to methods that expect generic types unless there is a very specific use case.

While it is true that Java's generic types are stored at runtime as just `Object` values, this does not mean it is okay to pass or expect `Object` instead of the correct type.

Java only allows one to do so by casting the receiver type of the called method to a "raw" non-generic version first.


## Bad Practice

```java
HashMap<String, Integer> hs = new HashMap<>();

((HashMap)hs).put(new Object(), 3);
// OR
((HashMap)hs).put((Object) "newkey", 3);
```
Note that to force this code to work, `hs` needed to first be cast to a "raw" `HashMap` type before we could abuse it.

Even if the type of the argument passed at runtime is correct, making the value's type `Object` will increase the chances of a bad cast or some other unsupported operation occurring later on.


## Recommended
Use only the intended generic type when using generics.


```java
hs.put("String1", 3);
```

## Exceptions
If such code is completely intentional and is accomplishing some specific goal, it may be safe to ignore this issue.

