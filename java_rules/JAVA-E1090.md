# Arguments to `Collections.nCopies()` should be in the correct order
**ID:** `JAVA-E1090` | **Lien:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-E1090)

![Critical](https://img.shields.io/badge/severity-critical-red) ![Bug Risk](https://img.shields.io/badge/type-bug_risk-green)

Passing the arguments of [ `Collections.nCopies(int, T)` ](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/util/Collections.html#nCopies(int,T)) in the wrong order can lead to unexpected behavior and incorrect results.


## Bad Practice

```java
// Fails to compile!
List<Character> list = Collections.nCopies('A', 3);
```
In the code above, the arguments are passed in the wrong order. The first argument should be the number of copies to make, and the second argument should be the element to make copies of. Here, however, the first argument is `'A'` and the second argument is an integer, `3` . In this case, `nCopies` is given a character as its first argument, which is promoted to an integer ( `A` is `65` as an `int` ), and an integer `3` as its second argument. The call would thus produce a `List<Integer>` value.

The reason for the compilation failure is the incompatibility of the element type `nCopies` infers, and the element type required by the variable itself. `nCopies` returns a `List<Integer>` , but `list` expects a `List<Character>` .


## Recommended
Change the order of the arguments.


```java
List<Character> list = Collections.nCopies(3, 'A');

assertEquals(3, list.size()); // passes.
```
