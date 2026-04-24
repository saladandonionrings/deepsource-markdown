# Audit required: Starting a subprocess
**ID:** `BAN-B606` | **Lien:** [DeepSource](https://deepsource.com/directory/python/issues/BAN-B606)

![Minor](https://img.shields.io/badge/severity-minor-yellow) ![Security](https://img.shields.io/badge/type-security-red)

Spawning of a subprocess in a way that doesn't use a shell is generally safe, but it maybe useful for penetration testing workflows to track where external system calls are used.

Python possesses many mechanisms to invoke an external executable. However, doing so may present a security issue if appropriate care is not taken to sanitize any user provided or variable input.


```python
import os

# Creating subprocess:
# The following calls can be sensitive if the command is not sanitized, since they are starting a subprocess.
os.spawnl(mode, path, *cmd)
os.spawnle(mode, path, *cmd, env)
os.spawnlp(mode, file, *cmd)
os.spawnlpe(mode, file, *cmd, env)
os.spawnv(mode, path, cmd)
```
