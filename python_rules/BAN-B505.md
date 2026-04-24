# Detected use of a weak cryptographic key
**ID:** `BAN-B505` | **Lien:** [DeepSource](https://deepsource.com/directory/python/issues/BAN-B505)

![Major](https://img.shields.io/badge/severity-major-orange) ![Security](https://img.shields.io/badge/type-security-red)

Key length, should provide enough entropy against brute-force attacks. Due to increase in computational power, cryptographic algorithms when used with smaller length keys have become susceptible.

Recommended key lengths for different algorithms:

* RSA and DSA algorithms is 2048 and higher. Keys with 1024 bits and below are now considered breakable.
* EC key length sizes are recommended to be 224 and higher with 160 and below considered breakable.
* For RSA public key exponent should be at least 65537.


## Bad practice

```python
from cryptography.hazmat.primitives.asymmetric import rsa, ec, dsa

dsa.generate_private_key(key_size=1024, backend=backend) # Insecure. Key size is less than 2048
# Insecure. Key size is less than 2048, public exponent less than 65537
rsa.generate_private_key(public_exponent=888, key_size=1024, backend=backend)
ec.generate_private_key(curve=ec.SECT163R2, backend=backend) # Insecure. Key size is less than 224
``

### Recommended
```python
from cryptography.hazmat.primitives.asymmetric import rsa, ec, dsa

dsa.generate_private_key(key_size=2048)
rsa.generate_private_key(public_exponent=65537, key_size=65537)
ec.generate_private_key(curve=ec.SECT163R2)
```
