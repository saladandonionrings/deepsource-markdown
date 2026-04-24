# Audit: Starting a process with a partial executable path
**ID:** `BAN-B607` | **Lien:** [DeepSource](https://deepsource.com/directory/python/issues/BAN-B607)

![Minor](https://img.shields.io/badge/severity-minor-yellow) ![Security](https://img.shields.io/badge/type-security-red)

Python possesses many mechanisms to invoke an external executable. If the desired executable path is not fully qualified relative to the filesystem root then this may present a potential security risk.

In POSIX environments, the PATH environment variable is used to specify a set of standard locations that will be searched for the first matching named executable. While convenient, this behavior may allow a malicious actor to exert control over a system. If they are able to adjust the contents of the PATH variable, or manipulate the file system, then a bogus executable may be discovered in place of the desired one. This executable will be invoked with the user privileges of the Python process that spawned it, potentially a highly privileged user.

This test will scan the parameters of all configured Python methods, looking for paths that do not start at the filesystem root, that is, do not have a leading ‘/’ character.


## Bad practice

```python
import subprocess

subprocess.run(['calculator', '-u', 'critical', msg], check=True) # Sensitive, Path not qualified from root
```

## Recommended

```python
import subprocess

subprocess.run(['/usr/bin/calculator', '-u', 'critical', msg], check=True) # Path qualified from root
```
