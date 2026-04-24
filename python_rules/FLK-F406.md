# `from module import *` is only allowed at module level
**ID:** `FLK-F406` | **Lien:** [DeepSource](https://deepsource.com/directory/python/issues/FLK-F406)

![Major](https://img.shields.io/badge/severity-major-orange) ![Bug Risk](https://img.shields.io/badge/type-bug_risk-green)

Importing `*` is generally discouraged, but if there's a need regardless, such imports should only be done on module level.

