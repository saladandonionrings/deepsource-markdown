# Audit required: Use of `marshal` module
**ID:** `BAN-B302` | **Lien:** [DeepSource](https://deepsource.com/directory/python/issues/BAN-B302)

![Major](https://img.shields.io/badge/severity-major-orange) ![Security](https://img.shields.io/badge/type-security-red)

The [marshal](https://docs.python.org/3/library/marshal.html) module is not intended to be secure against erroneous or maliciously constructed data. Never unmarshal data received from an untrusted or unauthenticated source.

Data serialization is the process of converting structured data to a format that allows sharing or storage of the data in a form that allows recovery of its original structure.

The marshal module exists mainly to support reading and writing the pseudo-compiled code for Python modules of .pyc files. Backward compatibility of the module is not guaranteed. `marshal` supports only elementary data types (e.g., dictionaries, lists, tuples, numbers, and strings).

It is recommended to avoid use of `marshal` module. Consider using [ `PyYaml` ](https://pyyaml.org/wiki/PyYAMLDocumentation) module with default safe loader. PyYAML is a YAML parser and emitter for Python. YAML is language-agnostic and human-readable serialization format and can represent simple data types & structures. But `marshal` module is much faster than YAML. If speed is a concern, use the [pickle](https://docs.python.org/3/library/pickle.html) module instead – the performance is comparable, version independence is guaranteed, and pickle supports a substantially wider range of objects than `marshal` .


## Bad practice

```python
from flask import request

import marshal

@app.route('/marshal')
def load():
    data = request.GET.get("data")
    conf = marshal.loads(data) # Insecure. Avoid using marhsal module
```

## Recommended

```python
from flask import request

import yaml

@app.route('/yaml')
def load():
    data = request.GET.get("data")
    conf = yaml.load(data) # Secure
```
