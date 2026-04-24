# Test for object identity should be 'is not'
**ID:** `FLK-E714` | **Lien:** [DeepSource](https://deepsource.com/directory/python/issues/FLK-E714)

![Minor](https://img.shields.io/badge/severity-minor-yellow) ![Style](https://img.shields.io/badge/type-style-blue)

Tests for object identity should use the form `x is not None` rather than `not x is None` . The former example is more readable.


## Not preferred:

```python
value = get_result()
if not value is None:
    print(value)
```

## Preferred:

```python
value = get_result()
if value is not None:
    print(value)
```
