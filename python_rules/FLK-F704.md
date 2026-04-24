# `yield` or `yield from` statement used outside of a function
**ID:** `FLK-F704` | **Lien:** [DeepSource](https://deepsource.com/directory/python/issues/FLK-F704)

![Critical](https://img.shields.io/badge/severity-critical-red) ![Bug Risk](https://img.shields.io/badge/type-bug_risk-green)

`yield` should not be used outside of a function. This will raise a `SyntaxError`

