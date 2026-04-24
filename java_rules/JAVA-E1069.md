# Enum fields should not be mutable
**ID:** `JAVA-E1069` | **Link:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-E1069)

![Major](https://img.shields.io/badge/severity-major-orange)![Bug Risk](https://img.shields.io/badge/type-bug_risk-green)

Avoid declaring public, non-final fields within enums.

Enums are special classes in Java. When one declares the variants of an enum, they are actually declaring singleton instances of that enum.

The reason one cannot create new instances of an enum is that only the set of instances representing each variant of that enum are allowed to exist at any point.


## Bad Practice
Consider the following enum:


```java
enum SomeEnum {
    A("222"),
    B("5555"),
    C("54465");

    public String pubField;

    SomeEnum(String data) {
        pubField = data;
    }
}
```
This enum has one public field, `pubField`. Now, consider what happens if we make use of this enum's variants:


```java
SomeEnum var1 = SomeEnum.A;

// prints "222"
System.out.println(var1.pubField);

var1.pubField = "otherValue";

// prints "otherValue"!
System.out.println(SomeEnum.A.pubField);
```
Modifying `pubField` from `var1` affects both the declaration as well as every other usage of `A`. This is because each enum variant is a singleton of its class, and only instances associated with variants can exist.


## Recommended
Declare enum fields as final. If you require mutability within enums for whatever reason, use appropriate synchronization to ensure that any change to the enum field is thread-safe.


```java
enum SomeEnum {
    A("222"),
    B("5555"),
    C("54465");

    private final Object LOCK = new Object();
    private String field;

    SomeEnum(String data) {
        field = data;
    }

    /**
     * Update one variant's value in a thread safe manner.
     */
    public void updateField(String newVal) {
        synchronized(LOCK) {
            field = newVal;
        }
    }
}
```
