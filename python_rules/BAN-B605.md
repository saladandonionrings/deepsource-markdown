# Starting a process with a shell detected
**ID:** `BAN-B605` | **Lien:** [DeepSource](https://deepsource.com/directory/python/issues/BAN-B605)

![Major](https://img.shields.io/badge/severity-major-orange) ![Security](https://img.shields.io/badge/type-security-red)

Spawning of a subprocess using a command shell is dangerous as it is vulnerable to various [shell injection attacks](https://owasp.org/www-community/attacks/Command_Injection) . Great care should be taken to sanitize all input in order to mitigate this risk. Calls of this type are identified by the use of certain commands which are known to use shells.

It is possible for an attacker to craft inputs to issue different commands than the ones you intended such as removing a file.

It is recommended to use functions that don't spawn a shell. If you must use them, use [ `shlex.quote` ](https://docs.python.org/3/library/shlex.html#shlex.quote) to sanitize the input by changing it to the shell-escaped version.


## Bad practice

```python
import os

# Malicious input
filename = "file.py; echo foo"

# Executing command in a shell without escaping. This will also run `echo foo`.
os.system("git add " + filename)
```

## Recommended

```python
import os, shlex

# Malicious input
filename = "file.py; echo foo"

# This ensures someone can't inject other commands into the given command.
os.system("git add " + shlex.quote(filename))
```
