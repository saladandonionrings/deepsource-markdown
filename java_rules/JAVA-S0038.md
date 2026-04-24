# Empty zip file entries should not be created
**ID:** `JAVA-S0038` | **Lien:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-S0038)

![Major](https://img.shields.io/badge/severity-major-orange) ![Anti-pattern](https://img.shields.io/badge/type-anti_pattern-purple)

The code calls `putNextEntry()` , immediately followed by a call to `closeEntry()` . This results in an empty `ZipFile` entry.

The contents of the entry should be written to the `ZipFile` between the calls to `putNextEntry()` and `closeEntry()` . This may cause issues with other software that does not expect empty entries in zip files generated in such a way.

