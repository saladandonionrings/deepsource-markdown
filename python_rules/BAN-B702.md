# Use of insecure `mako` templates detected
**ID:** `BAN-B702` | **Lien:** [DeepSource](https://deepsource.com/directory/python/issues/BAN-B702)

![Major](https://img.shields.io/badge/severity-major-orange) ![Security](https://img.shields.io/badge/type-security-red)

Mako has no environment wide variable escaping mechanism which leaves application vulnerable to [XSS attacks]( [https://owasp.org/www-project-top-ten/2017/A7_2017-Cross-Site_Scripting_(XSS)](https://owasp.org/www-project-top-ten/2017/A7_2017-Cross-Site_Scripting_(XSS)) .

Unlike [Jinja2](https://jinja.palletsprojects.com/en/3.0.x/) (an alternative templating), Mako has no environment wide variable escaping mechanism. Because of this, all input variables must be carefully escaped before use to prevent possible vulnerabilities to Cross Site Scripting (XSS) attacks.

[Autoescaping](https://flask.palletsprojects.com/en/1.1.x/templating/#controlling-autoescaping) is the concept of automatically escaping special characters. Special characters for HTML, XML and XHTMl are &, >, <, " as well as '. These characters carry specific meanings so need to be replaced by so called `entities` if you want to use them for text. Not doing so makes application susceptible to Cross Site Scripting (XSS) attacks.

It is recommended to use Jinja2 instead of Mako. When configuring the Jinja2 environment, the option to use autoescaping on input can be specified. When enabled, Jinja2 will filter input strings to escape any HTML content submitted via template variables.


## Bad practice

```python
from mako.template import Template

template = Template(filename="mytemplate.html") # Insecure. Using mako template
```

## Recommended

```python
from jinja2 import Environment, PackageLoader, select_autoescape
env = Environment(autoescape=True)

template = env.get_template("mytemplate.html")
```
