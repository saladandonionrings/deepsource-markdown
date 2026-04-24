# Use of `_create_unverified_context` detected
**ID:** `BAN-B323` | **Lien:** [DeepSource](https://deepsource.com/directory/python/issues/BAN-B323)

![Major](https://img.shields.io/badge/severity-major-orange) ![Security](https://img.shields.io/badge/type-security-red)

By default, Python will create a secure, verified ssl context for use in such classes as `HTTPSConnection` . However, it still allows using an insecure context via the `_create_unverified_context` that reverts to the previous behavior that does not validate certificates or perform hostname checks.

It is recommended to replace this call with the default HTTPS context.

