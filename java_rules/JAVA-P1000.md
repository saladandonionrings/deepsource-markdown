# Non-null boxed types are inefficient
**ID:** `JAVA-P1000` | **Lien:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-P1000)

![Major](https://img.shields.io/badge/severity-major-orange) ![Performance](https://img.shields.io/badge/type-performance-white)

Java's objects inherently add some overhead in terms of CPU and memory usage, and this overhead extends to boxed primitive types as well.

Avoid using the boxed object versions of primitives where they are not necessary.

When working with Java's primitives, treating them as objects can be useful at times (when using them as generic type arguments, for example). Boxed primitive types like `Integer` and `Byte` exist for this reason. These types combine the semantics of both objects and primitives; Arithmetic and logical operators apply to them as they do to primitives, but these object types also have methods such as `equals()` and `hashCode()` defined on them. They can also be `null` ; raw primitive types can never be null.

Most boxed types have very similar names to primitives (just capitalize the first letter), and while they are convenient, they come with a number of object related overheads:

* They occupy more space in memory ( [A header with a minimum of 8 bytes](https://stackoverflow.com/a/26416983/6325886) + the primitive value)
* They force the JVM to allocate unnecessarily when boxing primitives into their object versions.

Only create variables with boxed types if you require primitives to be nullable; they have little utility elsewhere.


## Bad Practice

```java
Integer i = 3; // i could have just been an int instead.
i += 1;
// ...
```

## Recommended
In the majority of cases, there is no need for boxed primitive types.


```java
int i = 3;
i += 1;
// ...
```
Boxed types are quite necessary if there is a chance that a primitive value may need to be nullable:


```java
String query = "SELECT someBool from someTable;";
PreparedStatement ps = dbConnection.prepareStatement(query);

ResultSet rs = ps.executeQuery();

while (rs.next()) {
    Boolean nullable = rs.getBoolean("someBool");

    if (nullable != null) {
        // ...
    }
}
```

## Exceptions
This issue is not raised for generic collections where boxed types are used as type parameters.

