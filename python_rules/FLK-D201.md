# No blank lines allowed before function docstring
**ID:** `FLK-D201` | **Lien:** [DeepSource](https://deepsource.com/directory/python/issues/FLK-D201)

![Minor](https://img.shields.io/badge/severity-minor-yellow) ![Documentation](https://img.shields.io/badge/type-documentation-lightgrey)

There shouldn't be any blank lines before the function docstring. Remove the blank lines to fix this issue.


```python
def example():

    '''Bad docstring.'''
    ...
```

```python
def example():
    '''Good docstring.'''
    ...
```
