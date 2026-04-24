# No blank lines allowed after function docstring
**ID:** `FLK-D202` | **Lien:** [DeepSource](https://deepsource.com/directory/python/issues/FLK-D202)

![Minor](https://img.shields.io/badge/severity-minor-yellow) ![Documentation](https://img.shields.io/badge/type-documentation-lightgrey)

There shouldn't be any blank lines after the function docstring. Remove the blank lines to fix this issue.


```python
def example():
    '''Bad docstring.'''

    pass
```

```python
def example():
    '''Good docstring.'''
    pass
```
