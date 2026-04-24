# Absolute paths should not be hard coded
**ID:** `JAVA-W0406` | **Link:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-W0406)

![Minor](https://img.shields.io/badge/severity-minor-yellow)![Anti-pattern](https://img.shields.io/badge/type-anti_pattern-purple)

Absolute paths may lead to portability issues.

This code constructs a `File` object using a hard coded absolute path name. This is not portable and may fail if the folder structure where the code is run is changed or the code is run on a different machine/OS.

Consider getting the value at runtime from a config file or from an environment variable instead of hard coding it at the use site.


## Bad Practice

```java
new File("/home/dannyc/workspace/j2ee/src/share/com/sun/enterprise/deployment");
```

## Recommended

```java
new File(config[Constants.DEPLOYMENT_PATH]); // Using runtime config as well as a constant defined in a dedicated constants class.
```
If this code runs in a containerized environment, this issue is less of a concern, but it would still be better to refactor the string into a separate string constants class, or a properties file which can be queried at run time.

