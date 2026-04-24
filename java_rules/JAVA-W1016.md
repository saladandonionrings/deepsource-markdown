# Method superfluously delegates to parent class method
**ID:** `JAVA-W1016` | **Link:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-W1016)

![Minor](https://img.shields.io/badge/severity-minor-yellow)![Anti-pattern](https://img.shields.io/badge/type-anti_pattern-purple)

This method appears to only call its superclass implementation, while directly passing its parameters to the super method. This method can be removed, as it provides no additional value.


## Bad Practice

```java
@Override
public String getName() {
    return super.getName();
}
```
This `getName` method is redundant, since the same behavior would occur even without explicitly overriding the parent method.


## Recommended
Remove the redundant overriding method. If this was not intended, and there is further logic to be implemented, consider marking this method with a `TODO` comment to ensure it is not missed in future work.


```java
@Override
public String getName() {
    // TODO: this method requires extra logic to be implemented.
    return super.getName();
}
```
