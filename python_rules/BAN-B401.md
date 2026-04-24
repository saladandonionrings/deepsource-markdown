# Telnet related module imported
**ID:** `BAN-B401` | **Lien:** [DeepSource](https://deepsource.com/directory/python/issues/BAN-B401)

![Major](https://img.shields.io/badge/severity-major-orange) ![Security](https://img.shields.io/badge/type-security-red)

A telnet-related module is being imported. Using `telnet` is considered insecure.

Consider the following points when using Telnet protocol:

* Telnet is a plain-text protocol, anyone watching your Telnet packets on the wire will see your username, password, and everything you do on the remote system.
* There are no authentication policies used in telnet causing huge security threat. Communication is carried out between the two desired hosts can be intercepted in the middle.

Use [ `ssh` ](https://www.ssh.com/academy/ssh/protocol) as an alternative to `telnet` . It protects user identities, passwords, and data from network snooping attacks, and allows secure logins and file transfers. Use of `telnet` has been replaced by `ssh` in almost all cases.


## Bad practice

```python
import telnetlib # Sensitive, Import of telnetlib

url = "telnet:// [[email protected]](/cdn-cgi/l/email-protection) "
connection = telnetlib.Telnet("somehost") # Sensitive, Using telnet protocol
```

## Recommended

```python
from paramiko import SSHClient

client = SSHClient()
url = "telnet:// [[email protected]](/cdn-cgi/l/email-protection) "
client.connect(url, username='user', password='secret')
```
