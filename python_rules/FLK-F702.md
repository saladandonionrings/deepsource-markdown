# `continue` statement outside of a `while` or `for` loop
**ID:** `FLK-F702` | **Lien:** [DeepSource](https://deepsource.com/directory/python/issues/FLK-F702)

![Critical](https://img.shields.io/badge/severity-critical-red) ![Bug Risk](https://img.shields.io/badge/type-bug_risk-green)

Using a `continue` statement outside of a `while` or `for` loop is a syntax error in Python. The `continue` statement can only be used inside loops to skip the rest of the current iteration and move to the next one.

The `continue` statement serves a specific purpose in loops:

* It skips the remaining code inside the current loop iteration
* Control flow jumps to the loop's condition check
* If the condition is true, the next iteration begins

Here's an example of incorrect usage:


```python
def process_data(value):
    if value < 0:
        continue  # SyntaxError: 'continue' not properly in loop
    return value * 2
```
The correct way would be to use `continue` within a loop:


```python
def process_numbers(numbers):
    results = []
    for value in numbers:
        if value < 0:
            continue  # Correctly skips negative numbers
        results.append(value * 2)
    return results
```
If you need to skip execution in a non-loop context, consider using:

* Early returns with `return`
* Conditional statements with `if` / `else`
* Function structure reorganization

