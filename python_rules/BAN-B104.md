# Audit: Binding to all interfaces detected with hardcoded values
**ID:** `BAN-B104` | **Lien:** [DeepSource](https://deepsource.com/directory/python/issues/BAN-B104)

![Major](https://img.shields.io/badge/severity-major-orange) ![Security](https://img.shields.io/badge/type-security-red)

Binding to all network interfaces can potentially open up a service to traffic on unintended interfaces, that may not be properly documented or secured. This can be prevented by changing the code so it explicitly only allows access from localhost.

When binding to `0.0.0.0` , you accept incoming connections from anywhere. During development, an application may have security vulnerabilities making it susceptible to SQL injections and other attacks. Therefore when the application is not ready for production, accepting connections from anywhere can be dangerous.

It is recommended to use `127.0.0.1` or local host during development phase. This prevents others from targeting your application and executing SQL injections against your project.


## Bad practice

```python
s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
s.bind(('0.0.0.0, 31137)) # Binding to all interfaces
```

## Recommended

```python
s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
s.bind(('127.0.0.1', 31137)) # Binding to local host
```

```python
# References:
- OWASP Top 10 2021 Category A05 - [Security Misconfiguration](https://owasp.org/Top10/A05_2021-Security_Misconfiguration/)
```
