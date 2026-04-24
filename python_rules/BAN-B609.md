# Audit required: Possible wildcard injection in call: `subprocess.Popen`
**ID:** `BAN-B609` | **Lien:** [DeepSource](https://deepsource.com/directory/python/issues/BAN-B609)

![Major](https://img.shields.io/badge/severity-major-orange) ![Security](https://img.shields.io/badge/type-security-red)

The use of partially qualified paths may result in unintended consequences if an unexpected file or symlink is placed into the path location given. This becomes particularly dangerous when combined with commands used to manipulate file permissions or copy data off of a system.

Python provides a number of methods that emulate the behavior of standard Linux command line utilities. Like their Linux counterparts, these commands may take a wildcard “” character in place of a file system path. This is interpreted to mean “any and all files or folders” and can be used to build partially qualified paths, such as “/home/user/”.


## Bad practice

```python
import subprocess

subprocess.Popen("/bin/chown *") # Sensitive, unexpected file may be placed in location
```

## Recommended

```python
import subprocess

subprocess.Popen("/bin/chown /home/user/some_file") # Fixed path provided
```
