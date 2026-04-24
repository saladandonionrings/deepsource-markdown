# `getClass` should not be used with enums whose members have custom bodies
**ID:** `JAVA-E1106` | **Lien:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-E1106)

![Major](https://img.shields.io/badge/severity-major-orange) ![Bug Risk](https://img.shields.io/badge/type-bug_risk-green)

Enum variants with custom bodies are implemented as anonymous classes, and calling `getClass` on an enum variant with a body will return the Class instance corresponding to the anonymous class, not the enum itself.

Use the [ `getDeclaringClass()` ](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/lang/Enum.html#getDeclaringClass()) method to retrieve the actual enum type in such cases.

Consider the following example of an enum:


```java
enum MyEnum {
    VARIANT_ONE,
    VARIANT_TWO,
    // This declares an anonymous subclass of MyEnum!
    VARIANT_THREE {
        @Override
        public String toString() {
            return "This is variant three";
        }
    };
}
```

## Bad Practice

```java
MyEnum value = MyEnum.VARIANT_THREE;

Class<MyEnum> clazz = value.getClass(); // returns MyEnum$1.class
```

## Recommended
Use the [ `getDeclaringClass()` ](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/lang/Enum.html#getDeclaringClass()) method instead.


```java
Class<MyEnum> clazz = value.getDeclaringClass(); // returns MyEnum.class correctly.
```
