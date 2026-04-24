# Collections should not be added to themselves
**ID:** `JAVA-E1077` | **Lien:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-E1077)

![Major](https://img.shields.io/badge/severity-major-orange) ![Bug Risk](https://img.shields.io/badge/type-bug_risk-green)

A collection is added to itself. As a result, computing its hash code will trigger a recursive loop and eventually throw a `StackOverflowException` .

This is more likely to occur if the programmer does not make use of Java's compile time generic features and relies on casting objects retrieved instead. It can also happen if the collection is explicitly casted to its raw type when calling `add` .


## Bad Practice

```java
ArrayList addList = new ArrayList(10);

Integer add2List = 32;

// add2List was misspelled, but this code still compiles successfully!
addList.add(addList);
```
Such an action serves virtually no practical purpose.


## Recommended
Use generics when creating any collection variables.


```java
ArrayList<Integer> addList = new ArrayList<>(10);

Integer add2List = 32;

// add2List was misspelled, but this will now trigger a compiler error.
addList.add(addList);
```
