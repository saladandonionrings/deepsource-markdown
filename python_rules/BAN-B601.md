# Possible shell injection via Paramiko call
**ID:** `BAN-B601` | **Lien:** [DeepSource](https://deepsource.com/directory/python/issues/BAN-B601)

![Major](https://img.shields.io/badge/severity-major-orange) ![Security](https://img.shields.io/badge/type-security-red)

Paramiko is a Python library designed to work with the SSH2 protocol for secure (encrypted and authenticated) connections to remote machines. It is intended to run commands on a remote host. These commands are run within a shell on the target and are thus vulnerable to various [shell injection attacks](https://owasp.org/www-community/attacks/Command_Injection) .

Use of Paramiko's `exec_command` or `invoke_shell` methods should be reviewed to ensure that the input is sanitized. It is recommended to use [ `shlex.quote` ](https://pypi.org/project/shellescape/) to santize the input.


## Bad practice

```python
import paramiko

client = paramiko.SSHClient(...)
ret = client.exec_command(input)
```

## Recommended
import paramiko, shlex

client = paramiko.SSHClient(...) ret = client.exec_command(shlex.quote(input))


```python
## References:
- [Paramiko library](http://www.paramiko.org/)
- [shlex](https://docs.python.org/3/library/shlex.html)
- [Shell injection attacks](https://owasp.org/www-community/attacks/Command_Injection)
- OWASP Top 10 2021 Category A03 - [Injection](https://owasp.org/Top10/A03_2021-Injection/)
- [CWE-77](https://cwe.mitre.org/data/definitions/77.html)
```
