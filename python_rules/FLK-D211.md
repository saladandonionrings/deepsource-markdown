# No blank lines allowed before class docstring
**ID:** `FLK-D211` | **Lien:** [DeepSource](https://deepsource.com/directory/python/issues/FLK-D211)

![Minor](https://img.shields.io/badge/severity-minor-yellow) ![Documentation](https://img.shields.io/badge/type-documentation-lightgrey)

There shouldn't be any blank lines before the class docstring. Remove the blank lines to fix this issue.


```python
class Example:

    '''Bad docstring.'''
```

```python
class Example:
    '''Good docstring.'''
```
