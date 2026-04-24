# Serializable class with non-serializable super class having no void constructor detected
**ID:** `JAVA-S0192` | **Link:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-S0192)

![Minor](https://img.shields.io/badge/severity-minor-yellow)![Bug Risk](https://img.shields.io/badge/type-bug_risk-green)

This class implements the Serializable interface and its superclass does not.

When such an object is deserialized, the fields of the superclass need to be initialized by invoking the void constructor of the superclass. Since the superclass does not have one, serialization and deserialization will fail at runtime.

If you have control over the code of the superclass, consider adding a void constructor to it.

