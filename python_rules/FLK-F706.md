# `return` statement used outside of a function or method
**ID:** `FLK-F706` | **Lien:** [DeepSource](https://deepsource.com/directory/python/issues/FLK-F706)

![Critical](https://img.shields.io/badge/severity-critical-red) ![Bug Risk](https://img.shields.io/badge/type-bug_risk-green)

A `return` statement should not be used outside of a function or method. This will raise a `SyntaxError` . Make sure that your code and indentation are correct.


## Bad practice

```python
def fibonacci(n):
    '''Returns n'th fibonacci number'''
    if n <= 1:
        return 1

return fibonacci(n-1) + fibonacci(n-2)  # Bad indentation, returning outside function
```

## Recommended

```python
def fibonacci(n):
    '''Returns n'th fibonacci number'''
    if n <= 1:
        return 1

    return fibonacci(n-1) + fibonacci(n-2)
```
