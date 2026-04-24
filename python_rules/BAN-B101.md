# Assert statement used outside of tests
**ID:** `BAN-B101` | **Lien:** [DeepSource](https://deepsource.com/directory/python/issues/BAN-B101)

![Major](https://img.shields.io/badge/severity-major-orange) ![Security](https://img.shields.io/badge/type-security-red)

Usage of `assert` statement in application logic is discouraged. `assert` is removed with compiling to optimized byte code. Consider raising an exception instead. Ideally, `assert` statement should be used only in tests.

Python has an option to compile the optimized bytecode and create the respective `.pyo` files by using the options `-O` and `-OO` . When used, these basic optimizations are done:

* All the assert statements are removed
* All docstrings are removed (when -OO is selected)
* Value of the `__debug__` built-in variable is set to `False`

It is recommended not to use `assert` in non-test files. A better way for internal self-checks is to check explicitly and raise respective error using an if statement.

Tip: Make sure `test_patterns` are defined in `.deepsource.toml` to avoid false-positives. Please check the [documentation](https://deepsource.io/docs/concepts/#test_patterns) to know more.

Consider this code snippet:


```python
def read_secret(self):
    assert self.is_admin, "You are unauthorized to read this"
    return self._secret
```
If `python` is run with the `-O` flag, the check for `self.is_admin` is completely ignored, which can cause secrets to be leaked.

This is how you can ensure the code always works:


```python
def read_secret(self):
    if not self.is_admin:
        raise AssertionError("You are unauthorized to read this")

    return self._secret
```
Here's a more detailed example. Consider the following script `foo.py` :


```python
import sys

def run():
    assert len(sys.argv)  == 5 # Insecure, statement will be removed when compiled to optimized byte code
    print("Argument variables are: ", sys.argv)

run()
```
When optimization is disabled:


```python
$ python foo.py 1 2 3 4 5
Traceback (most recent call last):
  File "foo.py", line 7, in <module>
    run()
  File "foo.py", line 4, in run
    assert len(sys.argv)  == 5 # Insecure, statement will be removed when compiled to optimized byte code
AssertionError
```
When optimization is enabled:


```python
$ python -O foo.pyo 1 2 3 4 5 6
Argument variables are:  ['foo.pyo', '1', '2', '3', '4', '5', '6']
```
Here, all the internal self-checks using the assert statements are removed, as we can see. Therefore, there's a chance for an application to behave strangely in this case. It is better do raise the Exception explicitly:


```python
import sys

def run():
    if not len(sys.argv)  == 5:
        raise ValueError
    print("Argument variables are: ", sys.argv)

run()
```
Note: During autofix, DeepSource will change the `assert` statements to `if` statements raising `AssertionError` . This is done to replicate the existing behavior.

