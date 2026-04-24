# Empty jar file entries should not be created
**ID:** `JAVA-S0039` | **Lien:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-S0039)

![Major](https://img.shields.io/badge/severity-major-orange) ![Anti-pattern](https://img.shields.io/badge/type-anti_pattern-purple)

The code calls `putNextEntry()` , immediately followed by a call to `closeEntry()` . This results in an empty `JarFile` entry.

The contents of the entry should be written to the `JarFile` between the calls to `putNextEntry()` and `closeEntry()` . This may cause issues when the jar file is executed.

