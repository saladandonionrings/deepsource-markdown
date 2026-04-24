# SSL used with bad defaults
**ID:** `BAN-B503` | **Lien:** [DeepSource](https://deepsource.com/directory/python/issues/BAN-B503)

![Minor](https://img.shields.io/badge/severity-minor-yellow) ![Security](https://img.shields.io/badge/type-security-red)

Python methods with default parameter values that specify the use of broken SSL/TLS protocol versions are insecure.

Several highly publicized exploitable flaws have been discovered in all versions of SSL and early versions of TLS. It is strongly recommended that the use of the following known broken protocol versions be avoided:

* SSL v2
* SSL v3
* TLS v1
* TLS v1.1

Ensure that a modern, strong protocol is used. All versions listed above are known to be vulnerable to attacks. Using [TLS v1.2](https://docs.python.org/3/library/ssl.html#ssl.PROTOCOL_TLSv1_2) is strongly recommended.


## Bad practice

```python
import ssl

def open_ssl_socket(version=SSL.SSLv2_METHOD): # Insecure. SSL v2 used
    pass
```

## Recommended

```python
import ssl

def open_ssl_socket(version=ssl.PROTOCOL_TLSv1_2): # Secure, TLS v1.2 used
    pass
```
