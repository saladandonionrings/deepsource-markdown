# No certificate validation detected for HTTP request
**ID:** `BAN-B501` | **Lien:** [DeepSource](https://deepsource.com/directory/python/issues/BAN-B501)

![Major](https://img.shields.io/badge/severity-major-orange) ![Security](https://img.shields.io/badge/type-security-red)

Disabling certificate validation for HTTP request leave application vulnerable to [man-in-the-middle](https://owasp.org/www-community/attacks/Manipulator-in-the-middle_attack) attacks.

When request methods are used, certificates are validated automatically which is the desired behavior. If certificate validation is explicitly turned off, requests will accept any TLS certificate presented by the server and will ignore hostname mismatches and/or expired certificates, which will make your application vulnerable to man-in-the-middle attacks.

Using TLS can greatly increase security by guaranteeing the identity of the party you are communicating with. This is accomplished by one or both parties presenting trusted certificates during the connection initialization phase of TLS.


## Bad practice

```python
import requests

requests.get('https://gmail.com', verify=False) # Insecure. No certificate validation
```

## Recommended

```python
import requests

requests.get('https://gmail.com', verify=True) # Secure. Certificate validation enabled.
requests.get('https://deepsource.io') # Secure. Certificate validation enabled by default
```
