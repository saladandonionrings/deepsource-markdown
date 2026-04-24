# File Transfer Protocol (FTP) related module imported
**ID:** `BAN-B402` | **Lien:** [DeepSource](https://deepsource.com/directory/python/issues/BAN-B402)

![Major](https://img.shields.io/badge/severity-major-orange) ![Security](https://img.shields.io/badge/type-security-red)

A FTP-related module is being imported. FTP is considered insecure. Use SSH/SFTP/SCP or some other encrypted protocol.

Consider the following points when using Telnet protocol:

* FTP is a plain-text protocol, anyone watching your ftp packets on the wire will see your username, password, and everything you do on the remote system. Data sent via FTP is vulnerable to sniffing, spoofing, and brute force attacks, among other basic attack methods.
* There are no authentication policies used in FTP causing huge security threat. Communication is carried out between the two desired hosts can be intercepted in the middle.

There are several common approaches to addressing these challenges and securing FTP usage. FTPS is an extension of FTP that can encrypt connections at the client’s request. Transport Layer Security (TLS), Secure Socket Layer (SSL), and SSH File Transfer Protocol (also known as Secure File Transfer Protocol or SFTP) are often used as more secure alternatives to FTP because they use encrypted connections.

[ `pysftp` ](https://pysftp.readthedocs.io/en/release_0.2.9/) is an easy to use sftp module that utilizes paramiko and pycrypto. It provides a simple interface to sftp. It is recommended to use `pysftp` over `ftblib` .


## Bad practice

```python
import ftplib

url = "ftp:// [[email protected]](/cdn-cgi/l/email-protection) "
connection = ftplib.FTP(url) # Sensitive, Using FTP protocol
```

## Recommended

```python
import pysftp

url = "ftp:// [[email protected]](/cdn-cgi/l/email-protection) "
connection = pysftp.Connection(host=url, username=username, password=password)
```
