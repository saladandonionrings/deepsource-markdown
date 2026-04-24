# Collection type is unsafely downcast to a concrete class
**ID:** `JAVA-E1037` | **Lien:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-E1037)

![Major](https://img.shields.io/badge/severity-major-orange) ![Bug Risk](https://img.shields.io/badge/type-bug_risk-green)

Attempting to cast a value held in an abstract collection type to a concrete type (casting `java.util.List` to `java.util.LinkedList` for example) may fail with a `ClassCastException` .

Avoid performing such casts, and instead consider using a more proper abstraction.

Do not assume the type of a collection as this means only one particular implementation will ultimately be usable.

If such casts are required, it may be a sign that your API needs to be restructured.


## Bad Practice
Here, `names` is stored in a `List` variable, but is used in a way more similar to a queue.


```java
List<String> names = getNames();
LinkedList<String> nameQueue = (LinkedList<String>)names;

String nextName = nameQueue.poll(); // poll is defined on LinkedList, and pops the first elements off.
```

## Recommended
Use the correct abstraction for the job. With regards to the example above, [ `java.util.Queue` ](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/util/Queue.html) is one of the interfaces `LinkedList` implements, and defines the `poll` method. Thus, to directly take advantage of it, you could use a `Queue` to store the returned value of `getNames()` :


```java
Queue<String> namesQueue = getNames();

String nextName = nameQueue.poll(); // poll is declared within the Queue interface.
```

## Exceptions
This issue will not be reported when an `instanceof` check for the variable's type is detected before a vulnerable type cast.

