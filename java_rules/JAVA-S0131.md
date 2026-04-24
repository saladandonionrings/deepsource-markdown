# Public static method returns freely modifiable array that may expose internal state
**ID:** `JAVA-S0131` | **Link:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-S0131)

![Major](https://img.shields.io/badge/severity-major-orange)![Security](https://img.shields.io/badge/type-security-red)

A public static method returns a reference to an array that is part of the static state of the class. Any code that calls this method can freely modify the underlying array.

This is dangerous because it could allow external code to modify the behavior of the class by changing data asssumed to be invariant.

One fix is to return a copy of the array, using `Arrays.copyOf(Object [])` for example.

