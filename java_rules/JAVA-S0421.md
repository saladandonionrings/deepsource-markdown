# No relationship between the generic parameter of the called method and its argument
**ID:** `JAVA-S0421` | **Lien:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-S0421)

![Critical](https://img.shields.io/badge/severity-critical-red) ![Bug Risk](https://img.shields.io/badge/type-bug_risk-green)

This call to a generic collection method contains an argument with an incompatible class from that of the collection's parameter (i.e., the type of the argument is neither a supertype nor a subtype of the corresponding generic type argument):


```java
HashMap<Integer, String> hm = new HashMap();

// ...

// Contrived, but still possible.
if (hm.contains(3.2)) {
    // ...
}
```
It is unlikely that the collection contains any objects that are equal to the method argument used here. Thus, such usage will inevitably fail, either with an exception if you are lucky, or in the worst case, unexpected behavior. Most likely, the wrong value is being passed to the method.

In general, instances of two unrelated classes are not equal. For example, if the `Foo` and `Bar` classes are not related by subtyping, then an instance of `Foo` should not be equal to an instance of `Bar` . Among other issues, doing so will likely result in an `equals` method that is not symmetrical.

The following class `Foo` overrides its `equals` method to allow for comparison with `Strings` .


```java
class Foo {

    @Override
    public boolean equals(Object o) {
        // for illustrative purposes.
        if (o instanceof String) return true; else return false;
    }
}
```
This `equals` method isn't symmetrical since a `String` can only be equal to a `String` . In certain cases this can be useful and may be a perfectly valid, if unclear, solution for the use case.

This behavior should not be relied on however, since it depends on an implementation detail of collection APIs. It is typically the case that when you check if a `Collection` contains a `Foo` , the `equals` method of the argument (e.g., the `equals` method of the `Foo` class) is used to perform the equality checks. This is not documented or guaranteed by any API docs though, and this assumption will not hold if the `Collection` concerned does not mirror what others do; no implementation is obligated to do so.

Ensure that you use variables of the proper type in the method call. If this is intentional, ensure that the collection being used supports this kind of usage (uses the `equals` method of the object we give to the method instead of the element `equals` method). You may want to consider refactoring this code to use a more well-supported or consistent way in case the collection's implementation changes.

