# Deprecated form of raising exception detected
**ID:** `FLK-W602` | **Lien:** [DeepSource](https://deepsource.com/directory/python/issues/FLK-W602)

![Major](https://img.shields.io/badge/severity-major-orange) ![Anti-pattern](https://img.shields.io/badge/type-anti_pattern-purple)

The `raise Exception, message` form of raising exceptions is deprecated in Python2, and removed in Python3. `raise Exception(message)` is the new form, and should be preferred.


## Bad practice

```python
raise ValueError, 'Expected "foo" to be > 0'
```

## Recommended

```python
raise ValueError('Expected "foo" to be > 0')
```
