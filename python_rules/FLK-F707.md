# `except:` is not the last exception handler
**ID:** `FLK-F707` | **Lien:** [DeepSource](https://deepsource.com/directory/python/issues/FLK-F707)

![Major](https://img.shields.io/badge/severity-major-orange) ![Bug Risk](https://img.shields.io/badge/type-bug_risk-green)

All exception handlers should be placed before an empty `except` block. A bare exception catcher will catch any exception it encounters making the exception handlers after it useless.


```python
try:
    some_func()

except Exception1:
    some_handler()

except:
    general_handler()

except Exception2:
    some_other_handler()
```

```python
try:
    some_func()

except Exception1:
    some_handler()

except Exception2:
    some_other_handler()

except:
    general_handler()
```
