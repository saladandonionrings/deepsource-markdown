# Detected subprocess `popen` call with shell equals `True`
**ID:** `BAN-B602` | **Lien:** [DeepSource](https://deepsource.com/directory/python/issues/BAN-B602)

![Major](https://img.shields.io/badge/severity-major-orange) ![Security](https://img.shields.io/badge/type-security-red)

Using `shell=True` can expose you to security risks if someone crafts input to issue different commands than the ones you intended.

Python possesses many mechanisms to invoke an external executable. However, doing so may present a security issue if appropriate care is not taken to sanitize any user-provided or variable input. Subprocess invocation using a command shell is dangerous as it is vulnerable to various [shell injection attacks](https://owasp.org/www-community/attacks/Command_Injection) . It is possible for an attacker to craft inputs to issue different commands than the ones you intended, for example: removing a file.

Great care should be taken to sanitize all input in order to mitigate this risk. Calls of this type are identified by a parameter of `shell=True` being given.

It is recommended to use functions that don't spawn a shell. If you must use them, use [ `shlex.quote` ](https://pypi.org/project/shellescape/) to sanitize the input.


## Bad practice

```python
import subprocess

subprocess.Popen(cmd, shell=True)  # Sensitive, shell spawned
```

## Recommended

```python
import subprocess

subprocess.Popen(cmd)
```
