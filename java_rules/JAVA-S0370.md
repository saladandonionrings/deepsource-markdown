# Class extends Servlet class and uses instance variables
**ID:** `JAVA-S0370` | **Lien:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-S0370)

![Critical](https://img.shields.io/badge/severity-critical-red) ![Bug Risk](https://img.shields.io/badge/type-bug_risk-green)

This class extends from a Servlet class, and uses an instance member variable. Since only one instance of a Servlet class is created by the J2EE framework, and it is used in a multithreaded way, this paradigm is highly discouraged and most likely problematic.

Consider only using method local variables, or implement proper synchronization on the static fields.

