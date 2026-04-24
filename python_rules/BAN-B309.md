# Use of `HTTPSConnection` may not be secure in Python versions < 2.7.9
**ID:** `BAN-B309` | **Lien:** [DeepSource](https://deepsource.com/directory/python/issues/BAN-B309)

![Major](https://img.shields.io/badge/severity-major-orange) ![Security](https://img.shields.io/badge/type-security-red)

Use of `HTTPSConnection` on older versions of Python prior to 2.7.9 and 3.4.3 does not provide many of the assurances one would expect when using SSL and leaves connections open to potential man-in-the-middle attacks.

A secure SSL session relies on validation of a `X.509 certificate` . Basic checks include:

* Certificate Authority trust verification
* Certificate revocation status
* Certificate expiration
* Certificate subject name matching

The `HTTPSConnection` class is used in a large number of locations and fails to check that certificates are signed by a valid authority. Without that check in place, the subsequent checks (some highlighted above) are largely invalid.

The result is that an attacker who has access to the network traffic between two endpoints relying on `HTTPSConnection` can trivially create a certificate that will be accepted by `HTTPSConnection` as valid - allowing the attacker to intercept, read and modify the traffic that should be encrypted by `SSL` .

It is recommended to update the Python version.


## Bad practice

```python
conn = httplib.HTTPSConnection(proxy_host, proxy_port)
```
