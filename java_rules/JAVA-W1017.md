# Type parameter shadows another type
**ID:** `JAVA-W1017` | **Lien:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-W1017)

![Major](https://img.shields.io/badge/severity-major-orange) ![Anti-pattern](https://img.shields.io/badge/type-anti_pattern-purple)

Avoid giving type parameters the same name as other types, or naming types with the same names as type parameters.

It is legal to name a type parameter anything, including the name of a pre-existing type in Java, and, though inadvisable, it is possible to name a class or interface with a single uppercase character, similar to the usual names of type parameters.

In either case, it is possible that a type may have the same name as a type parameter. This is not a good idea, because type parameters take precedence over declared types in scope.


## Bad Practice
Here, `java.lang.String` (which is always in scope) is shadowed by the type parameter `String` .


```java
public <String> void a(String s) {
    String s1 = s;
}
```
Similarly, types which have names similar to type parameters will be shadowed as well:


```java
class T {
    @Override
    public String toString() {
        return "T";
    }
}

// Here, T refers t the type parameter, not the local type.
<T> void a(T t) {
    T u = t;
}
```

## Recommended
Type parameters are by convention named as single capital letters, such as `T` , or `V` . Similarly, types are in general descriptively named, and should not be named like type parameters.

Ensure that type parameters do not have the same name as any imported or declared types in a file. Also ensure that you do not declare a type that could be shadowed by a type parameter; ensure that type names are descriptive.

