# Method uses the same code for two switch clauses
**ID:** `JAVA-S0412` | **Lien:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-S0412)

![Minor](https://img.shields.io/badge/severity-minor-yellow) ![Anti-pattern](https://img.shields.io/badge/type-anti_pattern-purple)

Method uses the same code for two switch clauses

This method uses the same code to implement two clauses of a switch statement.

This could be a valid usage for clarity's sake, but it might also indicate a coding mistake.


## Example

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
If this is intended, you can safely ignore this issue. Otherwise, this code could be simplified to use switch fall through to share the case block with multiple cases.

