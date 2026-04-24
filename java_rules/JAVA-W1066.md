# Method returning collection/array type returns `null` instead
**ID:** `JAVA-W1066` | **Lien:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-W1066)

![Major](https://img.shields.io/badge/severity-major-orange) ![Anti-pattern](https://img.shields.io/badge/type-anti_pattern-purple)

`null` should not be returned from methods that are supposed to return collection instances or arrays.

Returning `null` from such methods results in uglier code because now every caller has to check if the method has returned `null` . If the caller fails to do this, there's a chance that executing such code will result in a `NullPointerException` .

When dealing with collections or arrays, the absence of values can usually be communicated by returning an empty instance instead of `null` . This is much more convenient because this rids the caller from the responsibility of checking for `null` every time this method is called.


## Bad Practice

```java
public List<Integer> method() {
    // ... method body

    if (/* some condition */)
        return null;
}

public int[] m2() {
    // ... method body

    if (/* some condition */)
        return null;
}
```

## Recommended
Consider returning empty collection instances or arrays instead of `null` .


```java
public List<Integer> method() {
    // ... method body

    if (/* some condition */)
        return Collections.emptyList();
}

public int[] m2() {
    // ... method body

    if (/* some condition */)
        return new int[];
}
```
