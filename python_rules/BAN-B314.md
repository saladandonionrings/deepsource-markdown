# Use of an insecure method from `xml.etree.ElementTree` detected
**ID:** `BAN-B314` | **Lien:** [DeepSource](https://deepsource.com/directory/python/issues/BAN-B314)

![Major](https://img.shields.io/badge/severity-major-orange) ![Security](https://img.shields.io/badge/type-security-red)

Using various XML methods to parse untrusted XML data is known to be vulnerable to XML attacks. Using the [defusedxml](https://pypi.org/project/defusedxml) module is recommended. Methods should be replaced with their `defusedxml` equivalents.

The `xml.etree.ElementTree` module implements a simple and efficient API for parsing and creating XML data. But it makes the application vulnerable to:

* [Billion Laughs attack](https://en.wikipedia.org/wiki/Billion_laughs_attack) : It is a type of denial-of-service attack aimed at XML parsers. It uses multiple levels of nested entities. If one large entity is repeated with a couple of thousand chars repeatedly, the parser gets overwhelmed.
* [Quadratic blowup attack](https://www.acunetix.com/vulnerabilities/web/xml-quadratic-blowup-denial-of-service-attack/) : It is similar to a Billion Laughs attack. It abuses entity expansion, too. Instead of nested entities, it repeats one large entity with a couple of thousand chars repeatedly.

Replace vulnerable imports with the equivalent [defusedxml](https://pypi.org/project/defusedxml) package, or make sure `defusedxml.defuse_stdlib()` is called.


## Bad practice

```python
import xml.etree.ElementTree as ET
tree = ET.parse('some_fie.xml') # Use of method from etree.ElementTree
```

## Recommended

```python
from defusedxml.ElementTree import parse
tree = parse('some_fie.xml')
```
