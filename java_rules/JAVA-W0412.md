# Method uses the same code for two switch clauses
**ID:** `JAVA-W0412` | **Link:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-W0412)

![Minor](https://img.shields.io/badge/severity-minor-yellow)![Anti-pattern](https://img.shields.io/badge/type-anti_pattern-purple)

Method uses the same code for two switch clauses

This method uses the same code to implement two clauses of a switch statement.

This could be a valid usage for clarity's sake, but it might also indicate a coding mistake.


## Bad Practice

```java
// ...
  switch (c) {
  case 'b':
      buf.append('\b');
      where++;
      break;
  case 't':                 // First block
      buf.append('\t');
      where++;
      break;
  case 'n':
      buf.append('\n');
      where++;
      break;
  case 'f':                 // Second block is the same
      buf.append('\t');
      where++;
      break;
  // ...
```

## Recommended
If the duplication was intended, consider using case fallthrough:


```java
case 't':
case 'f':
    buf.append('\t');
    where++;
    break;
```
If you are on Java 12+, the new case syntax could also concisely achieve the same goal:


```java
case 't', 'f':
    buf.append('\t');
    where++;
    break;
```
If this is intended, you can safely ignore this issue.

