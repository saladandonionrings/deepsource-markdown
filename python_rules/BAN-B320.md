# Use of an insecure method from `lxml.etree`
**ID:** `BAN-B320` | **Lien:** [DeepSource](https://deepsource.com/directory/python/issues/BAN-B320)

![Major](https://img.shields.io/badge/severity-major-orange) ![Security](https://img.shields.io/badge/type-security-red)

Using various XML methods to parse untrusted XML data is known to be vulnerable to XML attacks. Using the [defusedxml](https://pypi.org/project/defusedxml) module is recommended. Methods should be replaced with their `defusedxml` equivalents.

