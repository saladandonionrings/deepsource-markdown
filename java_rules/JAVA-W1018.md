# Type bound extends final type
**ID:** `JAVA-W1018` | **Link:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-W1018)

![Minor](https://img.shields.io/badge/severity-minor-yellow)![Anti-pattern](https://img.shields.io/badge/type-anti_pattern-purple)

This type parameter appears to extend a final class, which is a useless operation. Just specify the class directly.

The only class which inherits from a final class is that class itself. Thus, just using the type directly is enough, there is no need to specify it as a type bound.


## Bad Practice
The method below accepts an argument with a generic type `T` that extends `java.lang.String`. However, `String` is a final class and cannot be extended further. This means that the only class that can be accepted by this method is `String` itself.


```java
<T extends String> void thing(T a) {
    System.out.println("a" + a);
}
```

## Recommended
Use the class directly.


```java
void thing(String a) {
    System.out.println("a" + a);
}
```
