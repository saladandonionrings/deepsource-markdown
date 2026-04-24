# Audit: Unsafe Jackson deserialization configurations should not be used
**ID:** `JAVA-A1024` | **Lien:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-A1024)

![Major](https://img.shields.io/badge/severity-major-orange) ![Security](https://img.shields.io/badge/type-security-red)

Using features such as `@JsonTypeInfo(use = JsonTypeInfo.Id.CLASS)` or `ObjectMapper.enableDefaultTyping()` with Jackson can be a security risk, as such configurations are stepping stones towards a successful exploit.

Jackson is a well known serialization/deserialization library for Java that supports deserializing data based solely on type information contained within it. This mechanism can be abused through "deserialization gadgets" to execute attacks on the target system.

Avoid specifying unsafe configurations for Jackson deserialization.

Jackson has faced a [number](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-4995) of [CVEs](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-19362) based on its [polymorphic type handling (PTH)](https://medium.com/@david.truong510/jackson-polymorphic-deserialization-91426e39b96a) or polymorphic deserialization as it was previously known. This feature allows the data itself to determine the type of object it will be deserialized into. While this can be convenient, it is susceptible to exploits known as "deserialization gadgets", which can lead to remote code execution attacks. Deserialization gadgets are specific classes which may allow an attacker to perform arbitrary operations or access private data during instantiation.


## Bad Practice
This isssue will be raised if a class uses the `@JsonTypeInfo` annotation with the `use` value set to `Id.CLASS` or `Id.MINIMAL_CLASS` .


```java
@JsonTypeInfo(use = Id.CLASS)
abstract class SomeClass {
    // ...
}
```
It will also be raised if a Jackson `ObjectMapper` instance has the `enableDefaultTyping()` method called on it:


```java
ObjectMapper mapper = new ObjectMapper();
mapper.enableDefaultTyping();
```

## Recommended
Use `@JsonTypeInfo(use = Id.NAME)` , along with `@JsonTypeName` as well as `JsonSubTypes` to allow polymorphic type handling.


```java
@JsonTypeInfo(use = JsonTypeInfo.Id.NAME, include = As.PROPERTY, property = "type")
@JsonSubTypes({
   @JsonSubTypes.Type(value = Square.class, name = "square"),
   @JsonSubTypes.Type(value = Circle.class, name = "circle")
})
class Shape {
    public String name;

    Shape(String name) {
      this.name = name;
   }
}

@JsonTypeName("square")
class Square extends Shape {
   public double length;

   Square() {
      this(null, 0.0);
   }

   Square(String name, double length) {
      super(name);
      this.length = length;
   }
}

@JsonTypeName("circle")
class Circle extends Shape {
   public double radius;

   Circle() {
      this(null, 0.0);
   }

   Circle(String name, double radius) {
      super(name);
      this.radius = radius;
   }
}
```
