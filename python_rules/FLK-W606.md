# `async` and `await` are reserved keywords starting with Python 3.7
**ID:** `FLK-W606` | **Lien:** [DeepSource](https://deepsource.com/directory/python/issues/FLK-W606)

![Major](https://img.shields.io/badge/severity-major-orange) ![Bug Risk](https://img.shields.io/badge/type-bug_risk-green)

Using these symbols as names can break your code in future versions of Python.

Renaming any variables named `async` or `await` in your code to something else will help in code migration later. You should rename the specific variable to something else.


## Bad practice

```python
def func(async=False):
    print(async)
```

## Recommended

```python
def func(is_async=False):
    print(is_async)
```
