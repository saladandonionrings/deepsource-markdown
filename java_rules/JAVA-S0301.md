# `Random` object is used only once, then discarded
**ID:** `JAVA-S0301` | **Lien:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-S0301)

![Major](https://img.shields.io/badge/severity-major-orange) ![Anti-pattern](https://img.shields.io/badge/type-anti_pattern-purple)

This code creates a `java.util.Random` object, uses it to generate one random number, and then discards the `Random` object. This produces mediocre quality random numbers and is inefficient. If possible, rewrite the code so that the `Random` object is created once and saved, and each time a new random number is required invoke a method on the existing `Random` object to obtain it.

If it is important that the generated Random numbers not be guessable, you must not create a new `Random` for each random number; this increases predictability. You should strongly consider using a `java.security.SecureRandom` instead (and avoid allocating a new `SecureRandom` for each random number needed).

