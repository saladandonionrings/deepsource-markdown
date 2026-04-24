# Use of deprecated function: `mktemp`
**ID:** `BAN-B306` | **Lien:** [DeepSource](https://deepsource.com/directory/python/issues/BAN-B306)

![Major](https://img.shields.io/badge/severity-major-orange) ![Security](https://img.shields.io/badge/type-security-red)

Creating and using insecure temporary files can leave application and system data vulnerable to attacks.

The insecure way is to first generate a file name with the `mktemp()` function and then create a file using this name. Unfortunately, this is not secure, because a different process may create a file with this name in the time between the call to `mktemp()` and the subsequent attempt to create the file by the first process. A malicious user can predict the name of the temporary file, resulting in other files being accessed, modified, corrupted, or deleted.

The solution is to combine the two steps and create the file immediately. This can be achieved using `NamedTemporaryFile()` or `mkstemp` .


## Bad practice

```python
import tempfile

filename = tempfile.mktemp() # Insecure
temp_file = open(filename, file)
```

## Recommended

```python
import tempfile

temp_file1 = tempfile.NamedTemporaryFile(delete=False) # Secure, replacement to tempfile.mktemp()
temp_file2 = tempfile.NamedTemporaryFile() # Secure, temporary file created will be automatically deleted
tempfile3 = tempfile.mkstemp() # Secure
```
