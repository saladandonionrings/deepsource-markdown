# Expected an indented block
**ID:** `FLK-E112` | **Lien:** [DeepSource](https://deepsource.com/directory/python/issues/FLK-E112)

![Major](https://img.shields.io/badge/severity-major-orange) ![Bug Risk](https://img.shields.io/badge/type-bug_risk-green)

This block of code should be indented.


## Bad practice

```python
for i in range(10):
print(i)  # Incorrect indentation
```

## Recommended

```python
for i in range(10):
    print(i)
```
