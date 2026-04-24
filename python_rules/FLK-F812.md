# List comprehension redefines name
**ID:** `FLK-F812` | **Lien:** [DeepSource](https://deepsource.com/directory/python/issues/FLK-F812)

![Minor](https://img.shields.io/badge/severity-minor-yellow) ![Bug Risk](https://img.shields.io/badge/type-bug_risk-green)

A list comprehension uses the same name as another variable defined in the module, which can lead to confusion. Either change the variable name in the list comprehension or change it in the module.


## Bad practice

```python
item = 12
values = [item*2 for item in range(10)]  # name `item` re-used
```

## Recommended

```python
item = 12
values = [num*2 for num in range(10)]  # Using `num` to avoid name clashes.
```
