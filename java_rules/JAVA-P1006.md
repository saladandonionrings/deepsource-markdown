# String concatenation using `+` within loops is costly and should be replaced by a `StringBuffer`/`StringBuilder`
**ID:** `JAVA-P1006` | **Lien:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-P1006)

![Major](https://img.shields.io/badge/severity-major-orange) ![Performance](https://img.shields.io/badge/type-performance-white)

The method seems to be building a `String` using concatenation in a loop. In each iteration, the `String` is converted to a `StringBuffer` / `StringBuilder` , appended to, and converted back to a `String` .

This can lead to a cost of `O(n^2)`` where n is the number of iterations, as the growing string is recopied in each iteration. This issue is detected only on Java versions 8 and below.


## Bad Practice

```java
String s = "";
for (int i = 0; i < field.length; ++i) {
    s = s + field[i];
}
```

## Recommended
Create a `StringBuffer` outside the loop, then update it from within.


```java
StringBuffer buf = new StringBuffer();
for (int i = 0; i < field.length; ++i) {
    buf.append(field[i]);
}
String s = buf.toString();
```
