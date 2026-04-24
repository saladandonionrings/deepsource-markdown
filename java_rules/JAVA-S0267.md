# Static initializer creates instance before all static final fields are assigned
**ID:** `JAVA-S0267` | **Lien:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-S0267)

![Critical](https://img.shields.io/badge/severity-critical-red) ![Bug Risk](https://img.shields.io/badge/type-bug_risk-green)

The class's static initializer creates an instance of the class before all of the static final fields are assigned.

This may easily lead to a scenario where the static instance of this class may be used while in a partially constructed state.

Reorder the creation of the static instance so that it occurs after all other fields (static or not) are initialized.

