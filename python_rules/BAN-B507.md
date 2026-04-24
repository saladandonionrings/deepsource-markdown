# Missing host key validation in SSH
**ID:** `BAN-B507` | **Lien:** [DeepSource](https://deepsource.com/directory/python/issues/BAN-B507)

![Major](https://img.shields.io/badge/severity-major-orange) ![Security](https://img.shields.io/badge/type-security-red)

Host keys are used to verify the identity of remote hosts when SSH protocol is used. Accepting unknown host keys leave the connection vulnerable to man-in-the-middle attacks.

Paramiko library provides the ability to set policy for missing hostkey using [MissingHostKeyPolicy](http://docs.paramiko.org/en/stable/api/client.html#paramiko.client.MissingHostKeyPolicy) method. By default, it uses [RejectPolicy](http://docs.paramiko.org/en/stable/api/client.html#paramiko.client.RejectPolicy) or automatically rejecting the unknown hostname & key.

Do not set the default missing host key policy for the Paramiko library to either `AutoAddPolicy` or `WarningPolicy` . When paramiko methods are used, host keys are verified by default.


## Bad practice

```python
from paramiko.client import SSHClient, AutoAddPolicy

client = SSHClient()
client.set_missing_host_key_policy(AutoAddPolicy) # Insecure. Host key validation policy set to AutoAddPolicy
client.connect(" [[email protected]](/cdn-cgi/l/email-protection) ")
```

## Recommended

```python
from paramiko.client import SSHClient

client = SSHClient()
client.connect(" [[email protected]](/cdn-cgi/l/email-protection) ") # Safe. Uses RejectPolicy by default
```
