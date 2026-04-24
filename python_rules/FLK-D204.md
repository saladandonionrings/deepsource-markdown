# 1 blank line required after class docstring
**ID:** `FLK-D204` | **Lien:** [DeepSource](https://deepsource.com/directory/python/issues/FLK-D204)

![Minor](https://img.shields.io/badge/severity-minor-yellow) ![Documentation](https://img.shields.io/badge/type-documentation-lightgrey)

There should be exactly one blank line after the class docstring.


```python
class example:
    """Bad docstring."""
    ...
```

```python
class  example:
    """Good docstring."""

    ...
```
