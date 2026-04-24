# Insecure `xmlrpclib` import detected
**ID:** `BAN-B411` | **Lien:** [DeepSource](https://deepsource.com/directory/python/issues/BAN-B411)

![Major](https://img.shields.io/badge/severity-major-orange) ![Security](https://img.shields.io/badge/type-security-red)

XMLRPC is particularly dangerous as it is also concerned with communicating data over a network. Use `defused.xmlrpc.monkey_patch()` function to monkey-patch xmlrpclib and mitigate remote XML attacks.

