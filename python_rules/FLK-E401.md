# Multiple imports on one line
**ID:** `FLK-E401` | **Lien:** [DeepSource](https://deepsource.com/directory/python/issues/FLK-E401)

![Minor](https://img.shields.io/badge/severity-minor-yellow) ![Style](https://img.shields.io/badge/type-style-blue)

Imports from different modules should be on different lines.


```python
import os, sys  # multiple imports on same line

# instead these imports should be on their own line
import os
import sys
```
