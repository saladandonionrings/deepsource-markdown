# Named lambda expression detected
**ID:** `FLK-E731` | **Lien:** [DeepSource](https://deepsource.com/directory/python/issues/FLK-E731)

![Minor](https://img.shields.io/badge/severity-minor-yellow) ![Anti-pattern](https://img.shields.io/badge/type-anti_pattern-purple)

Lambdas should not be assigned to a variable. Instead, they should be defined as functions. The primary reason for this is debugging. Lambdas show as `<lambda>` in tracebacks, where functions will display the function’s name.

