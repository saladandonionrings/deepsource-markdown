# Test for membership should be 'not in'
**ID:** `FLK-E713` | **Lien:** [DeepSource](https://deepsource.com/directory/python/issues/FLK-E713)

![Major](https://img.shields.io/badge/severity-major-orange) ![Anti-pattern](https://img.shields.io/badge/type-anti_pattern-purple)

Tests for membership should use the form `x not in the_list` rather than `not x in the_list` . The former example is simply more readable.


## Bad practice

```python
if not x in authors:
    authors.append(x)
```

## Recommended

```python
if x not in authors:
    authors.append(x)
```
