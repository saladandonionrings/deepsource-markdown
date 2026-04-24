# Use of `tempnam` detected
**ID:** `BAN-B325` | **Lien:** [DeepSource](https://deepsource.com/directory/python/issues/BAN-B325)

![Major](https://img.shields.io/badge/severity-major-orange) ![Security](https://img.shields.io/badge/type-security-red)

Use of `os.tempnam()` and `os.tmpnam()` is vulnerable to symlink attacks. Consider using `tmpfile()` instead.


## Bad practice

```python
import os

filename = os.tmpnam()
with open(filename, 'w') as f:
    # Do things with the file object
```

## Recommended

```python
import os

with os.tmpfile() as f:
    # Do things with the file object
```
