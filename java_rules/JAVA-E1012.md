# Objects should not be compared to themselves within assertions
**ID:** `JAVA-E1012` | **Lien:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-E1012)

![Major](https://img.shields.io/badge/severity-major-orange) ![Bug Risk](https://img.shields.io/badge/type-bug_risk-green)

This assertion appears to compare an object to itself. Such comparisons are not very useful, and are likely caused by a typo.

Check whether this was intended and correct the comparison if a mistake was made.


## Bad Practice

```java
SomeClass obj = ...;

assertEquals(obj, obj);

assertTrue(obj == obj);

assertThat(obj).hasSameHashCodeAs(obj);
```

## Recommended
Specify a different object to compare the value to.


```java
assertThat(expectedObj).hasSameHashCodeAs(obj);
```

## Exceptions
This issue does not trigger when it appears that the aim of the test is to verify the function of methods such as `hashCode` or `equals` . Test methods that are named with patterns such as `equal` , `hash_?code` or `object_?methods` (case insensitive) will not trigger this issue. E.g. performing self comparisons in a test with the name `testEquals` or `testObjectMethods` will not trigger this issue.

