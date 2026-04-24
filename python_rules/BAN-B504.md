# SSL used with no version specified
**ID:** `BAN-B504` | **Lien:** [DeepSource](https://deepsource.com/directory/python/issues/BAN-B504)

![Minor](https://img.shields.io/badge/severity-minor-yellow) ![Security](https://img.shields.io/badge/type-security-red)

Specific methods in Python's native SSL/TLS support and the pyOpenSSL module may use default versions of SSL/TLS protocol which are insecure.

Several highly publicized exploitable flaws have been discovered in all versions of SSL and early versions of TLS. It is strongly recommended that use of the following known broken protocol versions be avoided:

* SSL v2
* SSL v3
* TLS v1
* TLS v1.1

Ensure that a modern, strong protocol is used. All versions listed above are known to be vulnerable to attacks. Using [TLS v1.2](https://docs.python.org/3/library/ssl.html#ssl.PROTOCOL_TLSv1_2) is strongly recommended.


## Bad practice

```python
import ssl
import socket

ssl.wrap_socket(socket.socket())
```

## Recommended

```python
import ssl
import socket

ssl.wrap_socket(socket.socket(), ssl_version=ssl.PROTOCOL_TLSv1_2)
``

## References:
- [ssl module in python](https://docs.python.org/3/library/ssl.html#ssl.PROTOCOL_SSLv2)
- [TLS v1.2](https://docs.python.org/3/library/ssl.html#ssl.PROTOCOL_TLSv1_2)
- OWASP Top 10 2021 Category A02 - [Cryptographic Failures](https://owasp.org/Top10/A02_2021-Cryptographic_Failures/)
- OWASP Top 10 2021 Category A05 - [Security Misconfiguration](https://owasp.org/Top10/A05_2021-Security_Misconfiguration/)
- [CWE 327](https://cwe.mitre.org/data/definitions/327.html) - Use of a Broken or Risky Cryptographic Algorithm
```
