# Useless increment in return statement
**ID:** `JAVA-S0356` | **Link:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-S0356)

![Minor](https://img.shields.io/badge/severity-minor-yellow)![Anti-pattern](https://img.shields.io/badge/type-anti_pattern-purple)

This statement has a return statement that looks like `return x++;`. A postfix increment/decrement does not impact the value of the expression, so this increment/decrement has no effect.

Please verify that this statement does the right thing.

