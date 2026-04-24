# String.indexOf's arguments should not be reversed
**ID:** `JAVA-E1089` | **Lien:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-E1089)

![Major](https://img.shields.io/badge/severity-major-orange) ![Bug Risk](https://img.shields.io/badge/type-bug_risk-green)

When searching for a character in a string using [ `String.indexOf()` ](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/lang/String.html#indexOf(int,int)) , one should be careful to not mix up the order of its arguments.

One of the overloads of `indexOf()` takes two arguments; the first is the character to search for and the second is the index to start searching from. Both these parameters are integers, and this opens up new avenues for confusion due to [type promotion rules](https://docs.oracle.com/javase/specs/jls/se7/html/jls-5.html) .


## Bad Practice

```java
String s = "Hello, World!";

int index = s.indexOf(7, 'o'); // This is the wrong order!
```
In the example above, the arguments are passed in the wrong order. The first argument should be the character to search for, and the second should be the starting index to begin the search from. Here, however, the first argument is an integer, and the second argument is a character.

Due to type promotion, narrower types such as `char` will be converted automatically if a wider type (like `int` ) was expected. In this case, the method will return the index of the character represented by the integer value 7, rather than the index of the 'o' character.


## Recommended
Switch the arguments around.


```java
int index = s.indexOf('o', 7);
```
