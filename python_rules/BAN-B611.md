# Audit required: Potential SQL injection on `RawSQL` function
**ID:** `BAN-B611` | **Lien:** [DeepSource](https://deepsource.com/directory/python/issues/BAN-B611)

![Minor](https://img.shields.io/badge/severity-minor-yellow) ![Security](https://img.shields.io/badge/type-security-red)

Use of `extra` in Django querysets should be audited, since unsanitized strings can open up security vulnerabilities.

