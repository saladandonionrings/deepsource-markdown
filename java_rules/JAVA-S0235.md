# Object appears to have been created for no reason
**ID:** `JAVA-S0235` | **Link:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-S0235)

![Minor](https://img.shields.io/badge/severity-minor-yellow)![Anti-pattern](https://img.shields.io/badge/type-anti_pattern-purple)

Our analysis shows that this object is useless.

It's created and modified, but its value never goes outside the method or produces any side effect. Either there is a mistake and the object was intended to be used or it can be removed.

This analysis rarely produces false-positives. Common false-positive cases include:

* This object used to implicitly throw some obscure exception.* This object used as a stub to generalize the code.* This object used to hold strong references to weak/soft-referenced objects.
