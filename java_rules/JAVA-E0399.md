# Shift amounts outside the valid range may produce unexpected results
**ID:** `JAVA-E0399` | **Link:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-E0399)

![Major](https://img.shields.io/badge/severity-major-orange)![Anti-pattern](https://img.shields.io/badge/type-anti_pattern-purple)

The code performs a shift of an `int` or `long` by a constant amount outside the acceptable range. This could potentially cause overflow or other similar errors and is at best very confusing.

The Java Language Specification, Section [15.19](https://docs.oracle.com/javase/specs/jls/se11/html/jls-15.html#jls-15.19) has the following to say on this matter:

If the promoted type of the left-hand operand is int, then only the five lowest-order bits of the right-hand operand are used as the shift distance. It is as if the right-hand operand were subjected to a bitwise logical AND operator `&` ([§15.22.1](https://docs.oracle.com/javase/specs/jls/se11/html/jls-15.html#jls-15.22.1)) with the mask value `0x1f` (`0b11111`). The shift distance actually used is therefore always in the range `0` to `31`, inclusive.

If the promoted type of the left-hand operand is long, then only the six lowest-order bits of the right-hand operand are used as the shift distance. It is as if the right-hand operand were subjected to a bitwise logical AND operator `&` ([§15.22.1](https://docs.oracle.com/javase/specs/jls/se11/html/jls-15.html#jls-15.22.1)) with the mask value `0x3f` (`0b111111`). The shift distance actually used is therefore always in the range `0` to `63`, inclusive.


## Bad Practice
Consider the following shift operations on the `int` value `a`, and the `long` value `b`.


```java
int a = 2;

a << 40 == a << 8 // true, 40 % 32 = 8

a << 32 == a // true, 32 % 32 = 0

long b = 2;

b << 72 == b << 8 // true, 72 % 64 = 8

b << 64 == b // true, 64 % 64 = 0
```

## Recommended
The absolute shift amounts for `int` and `long` values must always be one less than the number of bits in their representation. That is, 64-bit values can only have shift amounts in the range -63, 63 while 32-bit values can only have shift amounts in the range of -31, 31.

