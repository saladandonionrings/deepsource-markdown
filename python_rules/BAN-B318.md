# Use of an insecure method from `xml.dom.minidom` detected
**ID:** `BAN-B318` | **Lien:** [DeepSource](https://deepsource.com/directory/python/issues/BAN-B318)

![Major](https://img.shields.io/badge/severity-major-orange) ![Security](https://img.shields.io/badge/type-security-red)

Using various XML methods to parse untrusted XML data is known to be vulnerable to XML attacks. Using the [defusedxml](https://pypi.org/project/defusedxml) module is recommended. Methods should be replaced with their `defusedxml` equivalents.

