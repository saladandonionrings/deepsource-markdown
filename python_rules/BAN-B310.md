# Audit required: Use of an insecure method method from `urllib` detected
**ID:** `BAN-B310` | **Lien:** [DeepSource](https://deepsource.com/directory/python/issues/BAN-B310)

![Major](https://img.shields.io/badge/severity-major-orange) ![Security](https://img.shields.io/badge/type-security-red)

`urllib` not only opens `http://` or `https://` URLs, but also `ftp://` and `file://` . With this, it might be possible to open local files on the executing machine which might be a security risk if the URL to open can be manipulated by an external user.

The `urllib.request` module defines functions and classes which help in opening URLs. `urllib.request.open` can open `ftp://` and `file://` URLs. This is usually not intended and makes the application vulnerable to [Server Side Request Forgery](https://owasp.org/www-community/attacks/Server_Side_Request_Forgery) attack. Performing requests from user-provided data could allow attackers to make requests on the internal network or change, retrieve or delete sensitive information. You are yourself responsible for validating the URL before opening it with `urllib` .

It is recommended to validate the user-provided data, such as the URL and headers used to construct the request.

Since this is an audit issue, some occurrences may be harmless here. The goal is to bring the issue in attention. Please make sure that the url is trusted. If the occurrences doesn't seem to be valid, please feel free to ignore them.


## Bad practice

```python
req = urllib.Request.request(url)
resp = urllib.request.urlopen(req)
```

## Recommended

```python
# Validate URL before opening it
if url.lower().startswith('http'):
  req = urllib.Request.request(url)
else:
  raise ValueError from None

with urllib.request.urlopen(req) as resp:
  [...]

## References:
- OWASP Top 10 2021 Category A10 - [Server Side Request Forgery](https://owasp.org/Top10/A10_2021-Server-Side_Request_Forgery_%28SSRF%29/)
- [CWE-918](https://cwe.mitre.org/data/definitions/918.html) - Server-Side Request Forgery (SSRF)
```
