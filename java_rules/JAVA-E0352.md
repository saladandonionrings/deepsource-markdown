# Overwriting a method parameter will not modify the original object
**ID:** `JAVA-E0352` | **Link:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-E0352)

![Major](https://img.shields.io/badge/severity-major-orange)![Bug Risk](https://img.shields.io/badge/type-bug_risk-green)

This method ignores the original value of a parameter and attempts to assign a new value to it.

This often indicates a mistaken belief that the write to the parameter will be conveyed back to the caller. Because a parameter is just a copy of a reference from the calling scope, overwriting it will only modify the method's local copy of the reference, not the calling scope's copy.

However, note that it is still possible to modify a value passed to a method if the value has mutable public fields or if the value exposes methods to modify its fields.

It may be helpful to keep in mind that Java's method parameters are passed by **value**, not reference. Primitive values are copied into the method arguments, and object references (not objects) are similarly copied. Method arguments merely increase the reference count for any objects.

Do not assign a new value to a parameter reference; it will not affect the original value.


## Bad Practice

```java
void method(Float param) {

    param = 3.2f; // Will not affect value of param in the calling scope.

    // ...
}
```
Note that in the following case, the value referenced by the parameter is modified, not the parameter itself (which is just a reference):


```java
void method1(HashMap<String, Integer> param) {
    // ...

    param.put("abc", 3); // Modifies the object pointed to by param instead of the reference itself.

    // ...
}
```

## Recommended
Just create a new variable. If you need to modify a value passed into the method, do so at the call site instead of within the method.


```java
float method1(float other) {

    float res = ...; // calculations...

    return res;
}

// ...

float input = ...;
float res = method1(input);
```
If you need to modify several values that are specified as inputs, consider creating a POJO (Plain Old Java Object) which can hold modifiable references:


```java
class MethodData {
    public float priceCorrection;
    public HashMap<String, Item> items;
};

// ...

MethodData method1(String input) {

    // ...

    MethodData data = ...; // Assign the value somehow

    return data;
}
```
