# Private method is never called
**ID:** `JAVA-S0324` | **Lien:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-S0324)

![Minor](https://img.shields.io/badge/severity-minor-yellow) ![Anti-pattern](https://img.shields.io/badge/type-anti_pattern-purple)

This private method is never called. Although it is possible that the method will be invoked through reflection, it is more likely that the method is never used, and should be removed.

Unless this method is intended to be used with reflection, it is recommended to remove it to increase code clarity.

If this method is intended to be called through reflection, ensure that the reflective access is implemented correctly.

