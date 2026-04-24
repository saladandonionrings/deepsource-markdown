# Multi-line docstring closing quotes should be on a separate line
**ID:** `FLK-D209` | **Lien:** [DeepSource](https://deepsource.com/directory/python/issues/FLK-D209)

![Minor](https://img.shields.io/badge/severity-minor-yellow) ![Documentation](https://img.shields.io/badge/type-documentation-lightgrey)


```python
def foo():
    """This is an example docstring.
    To show how to write multi-line docstring."""
```

```python
def foo():
    """
    This is an example docstring.
    To show how to write multi-line docstring.
    """
```
