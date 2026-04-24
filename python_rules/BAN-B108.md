# Hardcoded temporary directory detected
**ID:** `BAN-B108` | **Lien:** [DeepSource](https://deepsource.com/directory/python/issues/BAN-B108)

![Major](https://img.shields.io/badge/severity-major-orange) ![Security](https://img.shields.io/badge/type-security-red)

Using hardcoded temp directory is unsafe. The program can be tricked into performing file actions against the wrong file or using a malicious file instead of the expected temporary file. Prefer using [tempfile](https://docs.python.org/3/library/tempfile.html)

Malicious users can predict the file name and write to the directory containing the temporary file. They effectively hijack the temporary file by creating a symlink with the name of the temporary file before the program creates the file itself. This allows a malicious user to supply malicious data or cause actions by the program to affect the attacker chosen files.

[ `tempfile.TemporaryFile` ](https://docs.python.org/3/library/tempfile.html#tempfile.TemporaryFile) function should be used to safely create temporary files. Besides creating temporary files safely, it creates random filenames, which can not be predicted, and cleans up the file automatically.


## Bad practice

```python
with open('/tmp/abc', 'w') as f: # Insecure, Hard coded temporary directory used
    f.write('stuff')
```

## Recommended

```python
import tempfile

# Secure, temporary file is created using tempfile.TemporaryFile
# File will be deleted on close
with tempfile.TemporaryFile() as tmp:
    tmp.write('stuff')
```
