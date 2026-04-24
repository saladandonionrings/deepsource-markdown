# Storing an externally mutable value into a private static field may expose internal state
**ID:** `JAVA-S0134` | **Lien:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-S0134)

![Major](https://img.shields.io/badge/severity-major-orange) ![Security](https://img.shields.io/badge/type-security-red)

This code stores a reference to an externally mutable object into a static field. If unchecked changes to the mutable object would compromise security or other important properties, you will need to do something different.

It may be possible for external code to inspect or change the value of the static field by holding a reference to it after passing it to this class.

Storing a copy of the object is the better approach in many situations.

