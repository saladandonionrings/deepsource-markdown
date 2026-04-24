# Switch blocks must not contain statement labels
**ID:** `JAVA-E1005` | **Link:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-E1005)

![Critical](https://img.shields.io/badge/severity-critical-red)![Anti-pattern](https://img.shields.io/badge/type-anti_pattern-purple)

This `switch` block mixes `case` directives with statement labels. This is confusing at best, and in the worst case, may introduce bugs in your code.

This may have occurred due to a missing `case` keyword before the label.

Labels are typically used as an easy way to break out of nested control structures. For example, breaking entirely out of a loop nested in a `switch` block is made quite easy with a label placed just outside the `switch` block:


```java
String string = new Scanner(System.in).next();
int index = new Scanner(System.in).nextInt();
outer: switch (index) {
    case 1:
        for (int i = index; i < string.length() - index; i++) {
            if (string.charAt(index + i) != 'a') break outer; // Break out of the outer switch as well.
        }
        if (string.charAt(index) == 'c') break;
        // else, fallthrough.
    case 2: // ...
        System.out.println("dfdsf");
        break;
    // ...
    default:
};
```
According to the JLS, [Section 14.7](https://docs.oracle.com/javase/specs/jls/se16/html/jls-14.html#jls-14.7):

... There is no restriction against using the same identifier as a label and as the name of a package, class, interface, method, field, parameter, or local variable. ...


## Bad Practice

```java
switch (month) {
    case JANUARY:
        // ...
        break;
    case FEBRUARY:
        // ...
        break;
    MARCH: // !!!
        // ...
        break;
    case APRIL:
        // ...
        break;
    // ...
    default: // ...
};
```
In the switch block above, `MARCH` may have been intended as a value that would match against `month`. Unfortunately, because it was not marked as a `case` clause, `MARCH` will instead be interpreted as a statement label that appears as part of the `FEBRUARY` case clause.


```java
switch (month) {
    case JANUARY: // ...
    case FEBRUARY:
        loop: for (int i = MONDAY; i < SUNDAY; i++) {
            if (day == i) break loop;
        }
        // ...
   case MARCH: // ...
   // ...
};
```
In this example, the usage of the label `loop` is syntactically correct and there isn't much ambiguity surrounding its usage. However, this is still a bad practice. A label is not required to break only the inner `for` loop here; just using `break` directly would achieve the same effect. Labels are only effective when it is necessary to break out of multiple nested structures, and will otherwise clutter your code.


## Recommended
Make sure `switch` cases are properly formatted.


```java
switch (month) {
    case JANUARY:
        // ...
        break;
    case FEBRUARY:
        // ...
        break;
    case MARCH: // fixed.
        // ...
        break;
    case APRIL:
        // ...
        break;
    // ...
    default: // ...
};
```
If you have a `for` loop within a `switch` block and want to be absolutely sure that any `break` statements will properly break the inner for loop, you could consider putting it in a function that is called within the `switch` block:


```java
void processDay(int day) {
    for (int i = MONDAY; i < SUNDAY; i++) {
        if (day == i) break;
        // ...
    }
}

// ...

switch (month) {
    case JANUARY: // ...
    case FEBRUARY:
        processDay(day);
        // ...
   case MARCH: // ...
   // ...
};
```
