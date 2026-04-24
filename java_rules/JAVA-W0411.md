# Method uses the same code for multiple branches
**ID:** `JAVA-W0411` | **Lien:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-W0411)

![Minor](https://img.shields.io/badge/severity-minor-yellow) ![Anti-pattern](https://img.shields.io/badge/type-anti_pattern-purple)

This method seems to have the same code for multiple branch statements.

This could be a valid usage for clarity's sake, but it might also indicate a typo.


## Bad Practice

```java
if (someCondition) {
    System.out.println("a");
    System.out.println(1 + new Random().nextInt());
} else if (someOtherCondition) { // The else if block has the same content as the first if block...
    System.out.println("a");
    System.out.println(1 + new Random().nextInt());
} else if (new Random().nextBoolean()) {
    if ("3".equals("4")) System.out.println(3 + new Random().nextInt());
}
```

## Recommended
If the duplication was intended, consider just combining the conditions with an OR operator:


```java
if (someCondition || someOtherCondition) {
    System.out.println("a");
    System.out.println(1 + new Random().nextInt());
}
```
