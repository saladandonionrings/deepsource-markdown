# Insecure `lxml` import detected
**ID:** `BAN-B410` | **Lien:** [DeepSource](https://deepsource.com/directory/python/issues/BAN-B410)

![Minor](https://img.shields.io/badge/severity-minor-yellow) ![Security](https://img.shields.io/badge/type-security-red)

Using various methods to parse untrusted XML data is known to be vulnerable to XML attacks. Replace vulnerable imports with the equivalent [defusedxml](https://pypi.org/project/defusedxml) package.

