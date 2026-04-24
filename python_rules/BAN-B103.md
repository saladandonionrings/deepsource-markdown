# Insecure permissions set on a file
**ID:** `BAN-B103` | **Lien:** [DeepSource](https://deepsource.com/directory/python/issues/BAN-B103)

![Major](https://img.shields.io/badge/severity-major-orange) ![Security](https://img.shields.io/badge/type-security-red)

Files should be created with restrictive file permissions to prevent vulnerabilities such as information disclosure and code execution. In particular, any files which may contain confidential information should be set to only permit access by the owning user/service and group (i.e., no world/other access).

POSIX based operating systems utilize a permissions model to protect access to parts of the file system. Every file in the POSIX file system has the following permissions:

* Owner permissions - Determines what actions the owner of the file can perform on the file.
* Group permissions - Determines what actions a user who is a member of the group that a file belongs to can perform on the file.
* Other permissions - Determines what action all other users can perform on the file.

Granting permissions to `others` can lead to unintended access and modification to files. Discretion should be used when granting write access to files such as configuration files to prevent vulnerabilities, including denial of service and remote code execution.

It is recommended to assign the most restrictive permissions to files and directories.


## Bad practice

```python
import os


os.chmod('/etc/passwd', 0o227) # Insecure, read and write permission granted to others
os.chmod('~/.bashrc', 511) # Insecure, write permission granted to others
os.chmod('/etc/hosts', 0o777) # Insecure, write permission granted to group and others
```

## Recommended

```python
import os


os.chmod('/etc/passwd', 0o664)
os.chmod('~/.bashrc', 0o644)
os.chmod('/etc/hosts', 0o700)
```
* OWASP Top 10 2021 Category A5 - [Security Misconfiguration](https://owasp.org/Top10/A05_2021-Security_Misconfiguration/)
* [os.chmod](https://docs.python.org/3/library/os.html#os.chmod)
* CWE 732 - [Incorrect Permission Assignment for Critical Resource](https://cwe.mitre.org/data/definitions/732.html)

