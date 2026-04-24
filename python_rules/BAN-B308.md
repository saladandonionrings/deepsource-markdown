# Audit required: Use of `mark_safe` detected
**ID:** `BAN-B308` | **Lien:** [DeepSource](https://deepsource.com/directory/python/issues/BAN-B308)

![Major](https://img.shields.io/badge/severity-major-orange) ![Security](https://img.shields.io/badge/type-security-red)

Use of `mark_safe()` may expose [cross-site scripting (XSS)](https://owasp.org/www-project-top-ten/2017/A7_2017-Cross-Site_Scripting_(XSS)) vulnerabilities and should be reviewed. `mark_safe` explicitly marks a string as safe for (HTML) output purposes.

Django auto-escapes all output from template variable tags unless explicitly told not to. Use of `mark_safe()` function implies that the parameter is safe for client-side output without Django's automatic string escaping. It's a legitimate way of defining strings that are intended to be interpreted as HTML.

Using `mark_safe()` on an internally generated string is okay but becomes a security risk if used on unchecked user input.

Since this is an audit issue, some occurrences may be harmless here. The goal is to bring the issue to attention. Please make sure that the input string is trusted. If the occurrences don't seem to be valid, please feel free to ignore them.

When possible, use [format_html](https://docs.djangoproject.com/en/3.0/ref/utils/#django.utils.html.format_html) . It is safe as all arguments are passed through [conditional_escape()](https://docs.djangoproject.com/en/3.0/ref/utils/#django.utils.html.conditional_escape)


## Bad practice

```python
mark_safe("<b>%s</b> %s" % (user_input))
```

## Recommended

```python
format_html("<b>%s</b>, user_input)
```
