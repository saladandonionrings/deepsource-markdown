# Detected use of a bad version of `SSL`
**ID:** `BAN-B502` | **Lien:** [DeepSource](https://deepsource.com/directory/python/issues/BAN-B502)

![Major](https://img.shields.io/badge/severity-major-orange) ![Security](https://img.shields.io/badge/type-security-red)

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

ssl.wrap_socket(socket.socket(), ssl_version=ssl.PROTOCOL_SSLv2) # Insecure. SSL v2 used
```

## Recommended

```python
import ssl
import socket

ssl.wrap_socket(socket.socket(), ssl_version= ssl.PROTOCOL_TLSv1_2) # Secure, TLS v1.2 used
```
