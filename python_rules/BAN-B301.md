# Audit required: Use of `pickle` module
**ID:** `BAN-B301` | **Lien:** [DeepSource](https://deepsource.com/directory/python/issues/BAN-B301)

![Major](https://img.shields.io/badge/severity-major-orange) ![Security](https://img.shields.io/badge/type-security-red)

The [pickle](https://docs.python.org/3/library/pickle.html) module is not secure against erroneous or maliciously constructed data. Never unpickle data received from an untrusted or unauthenticated source.

Python's `pickle` module is used for serializing and de-serializing a Python object structure. Data serialization is the process of converting structured data to a format that allows sharing or storage of the data in a form that allows recovery of its original structure.

Insecure deserialization is when an application deserializes the data that it gets without any kind of validation, or even the authenticity of the data. It is easy to execute arbitrary code when unpickling data. Unpickling can be exploited to execute arbitrary commands on your machine.

If `pickle` is not absolutely necessary for the use-case, consider using a safer serialization, like [PyYaml](https://pyyaml.org/wiki/PyYAMLDocumentation) .PyYAML is a YAML parser and emitter for Python. YAML is language-agnostic and human-readable serialization format. But `pickle` has its advantages too. Pickle format is specific to Python and can represent a wide variety of data structures and objects where as YAML represents simple data types & structures in a language-portable manner.

Recommended practices when using `pickle` module:

* It should not be used between unknown parties. Unpickling data from malicious client can cause remote execution attack. Only unpickle data you trust.
* Communicating parties should have an encrypted network connection. Otherwise, data to be unserialized can be modified and thus unsafe to pickle.
* Consider signing data with [hmac](https://docs.python.org/3/library/hmac.html#module-hmac) if you need to ensure that it has not been tampered with. Pickle can be signed before storage or transmission, and its signature can be verified before loading it on the receiver side.

Refer to [this blog post](https://www.synopsys.com/blogs/software-security/python-pickling/) to know more about dangers of using `pickle` module.


## Bad practice

```python
from flask import request

import picke

@app.route('/pickle')
def load():
    data = request.GET.get("data")
    conf = pickle.load(data) # Insecure. Avoid using pickle
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
