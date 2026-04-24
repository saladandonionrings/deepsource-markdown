# raising `NotImplemented` is not allowed
**ID:** `FLK-F901` | **Lien:** [DeepSource](https://deepsource.com/directory/python/issues/FLK-F901)

![Major](https://img.shields.io/badge/severity-major-orange) ![Bug Risk](https://img.shields.io/badge/type-bug_risk-green)

While returning `NotImplemented` would be fine, raising it doesn't work and will cause a `TypeError` because `NotImplemented` is not an exception type.

`NotImplemented` signals to the runtime that it should ask someone else to satisfy the operation. In the expression `a == b` , if `a.__eq__(b)` returns `NotImplemented` , then Python tries `b.__eq__(a)` . If `b` knows enough to return `True` or `False` , then the expression can succeed. If it doesn't, the runtime will fall back to the built-in behavior (based on identity for `==` and `!=` ).


## Bad practice

```python
class MyClass:
    def __lt__(self, other):
        raise NotImplemented  # This will raise a TypeError, as NotImplemented cannot be raised
```

## Recommended
If you want to disallow comparison with this class completely:


```python
class MyClass:
    def __lt__(self, other):
        raise NotImplementedError  # Will raise the exception
```
If you want the comparison to be possible, and let Python figure out which comparison operations will be possible by default, you can `return NotImplemented` instead:


```python
class MyClass:
    def __lt__(self, other):
        return NotImplemented  # Will allow Python to figure out the fallback
```
