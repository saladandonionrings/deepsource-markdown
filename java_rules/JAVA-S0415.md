# Method superfluously delegates to parent class method
**ID:** `JAVA-S0415` | **Lien:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-S0415)

![Minor](https://img.shields.io/badge/severity-minor-yellow) ![Anti-pattern](https://img.shields.io/badge/type-anti_pattern-purple)

This derived method merely calls the same super class method passing in the exact parameters received.

This method can be removed, as it provides no additional value.

If this is intentional, you can silence this issue by adding a `skipcq` line to this file:


```java
// skipcq: JAVA-S0415
```
