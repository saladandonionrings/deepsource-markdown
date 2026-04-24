# Block comment should start with `# `
**ID:** `FLK-E265` | **Lien:** [DeepSource](https://deepsource.com/directory/python/issues/FLK-E265)

![Minor](https://img.shields.io/badge/severity-minor-yellow) ![Style](https://img.shields.io/badge/type-style-blue)

Block comments should have one space before the pound sign `#` and the comment itself.


## Bad practice

```python
#Save the data to DB, to ensure redundancy before responding
db.save(req.data)
api.send(req)
```

## Recommended

```python
# Save the data to DB, to ensure redundancy before responding
db.save(req.data)
api.send(req)
```
