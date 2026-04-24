# First line should not be the function’s “signature”
**ID:** `FLK-D402` | **Lien:** [DeepSource](https://deepsource.com/directory/python/issues/FLK-D402)

![Minor](https://img.shields.io/badge/severity-minor-yellow) ![Documentation](https://img.shields.io/badge/type-documentation-lightgrey)

The first line of the function docstring has the function's signature, which is not recommended. It shouldn't be specified unless you're specifying or explaining something which might be confusing. It is recommended to remove the redundant function signature from the docstring.


## Bad practice

```python
def area(a, b):
    '''area(a, b) -> a*b'''
    ...
```

## Recommended

```python
def area(a, b):
    '''Calculates area of rectagnle with sides a and b'''
    ...
```
