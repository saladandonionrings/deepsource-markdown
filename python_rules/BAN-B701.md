# Use of jinja2 templates with `autoescape=False` detected
**ID:** `BAN-B701` | **Lien:** [DeepSource](https://deepsource.com/directory/python/issues/BAN-B701)

![Critical](https://img.shields.io/badge/severity-critical-red) ![Security](https://img.shields.io/badge/type-security-red)

Using Jinja2 templates without autoescaping enabled leaves application vulnerable to [XSS attacks]( [https://owasp.org/www-project-top-ten/2017/A7_2017-Cross-Site_Scripting_(XSS)](https://owasp.org/www-project-top-ten/2017/A7_2017-Cross-Site_Scripting_(XSS)) .

[Autoescaping](https://flask.palletsprojects.com/en/1.1.x/templating/#controlling-autoescaping) is the concept of automatically escaping special characters. Special characters for HTML, XML and XHTMl are &, >, <, " as well as '. These characters carry specific meanings so need to be replaced by so called `entities` if you want to use them for text. Not doing so makes application susceptible to Cross Site Scripting (XSS) attacks.

When configuring the Jinja2 environment, the option to use autoescaping on input can be specified. By default, autoescaping is disabled. When enabled, Jinja2 will filter input strings to escape any HTML content submitted via template variables.


## Bad practice

```python
from jinja2 import Environment

template_env1 = Environment() # Insecure. Autoescape set to False by default
template_env2 = Environment(autoescape=False) # Insecure
```

## Recommended

```python
from jinja2 import Environment

template_env = Environment(autoescape=True) # Secure
```
