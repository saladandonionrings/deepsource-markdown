# Function call with `shell=True` parameter identified
**ID:** `BAN-B604` | **Lien:** [DeepSource](https://deepsource.com/directory/python/issues/BAN-B604)

![Major](https://img.shields.io/badge/severity-major-orange) ![Security](https://img.shields.io/badge/type-security-red)

Method calls for the presence of a keyword parameter `shell` equalling true can be vulnerable to [shell injection attacks](https://owasp.org/www-community/attacks/Command_Injection) . This issue is intended to catch custom wrappers to vulnerable methods that may have been created.

It is possible for an attacker to craft inputs to issue different commands than the ones you intended such as removing a file.

It is recommended to use functions that don't spawn a shell. If you must use them, use [ `shlex.quote` ](https://pypi.org/project/shellescape/) to sanitize the input.


## Bad practice

```python
cmd = '/bin/gcc --{}' % version
pop(cmd, shell=True)
```

## Recommended

```python
cmd = '/bin/gcc --{}' % version
pop(cmd)
```
