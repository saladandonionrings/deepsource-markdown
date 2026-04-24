# One-line docstring should fit on one line with quotes
**ID:** `FLK-D200` | **Lien:** [DeepSource](https://deepsource.com/directory/python/issues/FLK-D200)

![Minor](https://img.shields.io/badge/severity-minor-yellow) ![Documentation](https://img.shields.io/badge/type-documentation-lightgrey)

If a docstring fits in a single line (72 characters according to PEP8), it is recommended to have the quotes on the same line.


## Bad practice

```python
def foo():
  """
  Runs bar and returns baz.
  """
  baz = bar()
  return baz
```

## Recommended

```python
def foo():
  """Runs bar and returns baz."""
  baz = bar()
  return baz
```
