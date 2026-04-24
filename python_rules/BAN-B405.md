# Import of method(s) from `xml.etree` detected
**ID:** `BAN-B405` | **Lien:** [DeepSource](https://deepsource.com/directory/python/issues/BAN-B405)

![Minor](https://img.shields.io/badge/severity-minor-yellow) ![Security](https://img.shields.io/badge/type-security-red)

Using various methods to parse untrusted XML data is known to be vulnerable to XML attacks.

The xml.etree.ElementTree module implements a simple and efficient API for parsing and creating XML data. But it makes the application vulnerable to:

* [Billion Laughs attack](https://en.wikipedia.org/wiki/Billion_laughs_attack) - It is a type of denial-of-service attack aimed at parsers of XML documents. It uses multiple levels of nested entities. If one large entity is repeated with a couple of thousand chars over and over again, parser gets overwhelmend.
* [Quadratic blowup attack](https://www.acunetix.com/vulnerabilities/web/xml-quadratic-blowup-denial-of-service-attack/) - It is similar to a Billion Laughs attack. It abuses entity expansion, too. Instead of nested entities it repeats one large entity with a couple of thousand chars over and over again.

Replace vulnerable imports with the equivalent [defusedxml](https://pypi.org/project/defusedxml) package, or make sure `defusedxml.defuse_stdlib()` is called.


## Bad practice

```python
import xml.etree.ElementTree as ET # Insecure, import from xml.etree
tree = ET.parse('some_fie.xml')
```

## Recommended

```python
from defusedxml.ElementTree import parse
tree = parse('some_fie.xml')
```
