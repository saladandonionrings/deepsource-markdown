# Insecure library imported
**ID:** `BAN-B413` | **Lien:** [DeepSource](https://deepsource.com/directory/python/issues/BAN-B413)

![Major](https://img.shields.io/badge/severity-major-orange) ![Security](https://img.shields.io/badge/type-security-red)

Libraries like `crypto` and `pycrypto` are no longer actively maintained and has been deprecated in favor of pyca/cryptography library.

`pycrypto` library is also known to have publicly disclosed [buffer overflow vulnerability](https://github.com/dlitz/pycrypto/issues/176) .

