# Insecure `pycryptodome` library imported
**ID:** `BAN-B414` | **Lien:** [DeepSource](https://deepsource.com/directory/python/issues/BAN-B414)

![Major](https://img.shields.io/badge/severity-major-orange) ![Security](https://img.shields.io/badge/type-security-red)

pycryptodome is a direct fork of pycrypto that has not fully addressed the issues inherent in PyCrypto. It seems to exist, mainly, as an API compatible continuation of pycrypto and should be deprecated in favor of pyca/cryptography which has more support among the Python community.

