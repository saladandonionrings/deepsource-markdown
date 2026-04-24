# Unsafe usage of `yaml.load` function detected
**ID:** `BAN-B506` | **Lien:** [DeepSource](https://deepsource.com/directory/python/issues/BAN-B506)

![Major](https://img.shields.io/badge/severity-major-orange) ![Security](https://img.shields.io/badge/type-security-red)

It is not safe to call [yaml.load](https://pyyaml.org/wiki/PyYAMLDocumentation) with any data received from an untrusted source. The `yaml.load` function provides the ability to construct an arbitrary Python object, which may be dangerous if you receive a YAML document from an untrusted source.

Deserialization of untrusted data exposes application to:

* [Parameter tampering attacks](https://owasp.org/www-community/attacks/Web_Parameter_Tampering) : Manipulation of parameters exchanged between client and server in order to modify application data, such as user credentials and permissions, price and quantity of products, etc.
* [Remote code execution attacks](https://owasp.org/www-community/attacks/Command_Injection) : Structure of unserialized data is changed to modify the behavior of object being unserialized.

It is recommended to use `yaml.safe_load` . The function `yaml.safe_load` limits this ability to simple Python objects like integers or lists.


## Bad practice

```python
from flask import request

import yaml

@app.route('/yaml')
def load():
    data = request.GET.get("data")
    conf = yaml.load(data) # Insecure. Avoid using yaml.load
```

## Recommended

```python
from flask import request

import yaml

@app.route('/yaml')
def load():
    data = request.GET.get("data")
    conf = yaml.safe_load(data) # Secure
```
