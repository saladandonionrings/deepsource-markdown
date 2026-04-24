# XSSRequestWrapper must not be used
**ID:** `JAVA-S1007` | **Lien:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-S1007)

![Critical](https://img.shields.io/badge/severity-critical-red) ![Security](https://img.shields.io/badge/type-security-red)

`XSSRequestWrapper` is an `HTTPRequestWrapper` implementation that attempts to strip out potential XSS vulnerabilities from request data, and has been circulated through a [number](https://gist.github.com/madoke/2347047) [of](https://dzone.com/articles/stronger-anti-cross-site) [blogs](https://www.javacodegeeks.com/2012/07/anti-cross-site-scripting-xss-filter.html) over the years. Unfortunately its implementation suffers from a number of design defects. These render it a weak protection, and even contribute towards facilitating certain attacks.

Using `XSSRequestWrapper` is not recommended due to its defective design.

The filtering implemented by `XSSRequestWrapper` is weak for a few reasons:

* It covers only parameters, not headers and side-channel inputs
* The chain of replace functions can be bypassed easily (see example below)
* It's a black-list of very specific bad patterns (rather than a whitelist of good/valid input)

This issue will be raised if the presence of the XSSRequestWrapper class is detected in the codebase.

Typically, XSSRequestWrapper can catch and remove patterns such as the one below:


```java
<script>alert(1)</script>
```
Such strings will be entirely removed from the given input. However, the following input for example would behave rather differently:


```java
<scrivbscript:pt>alert(1)</scrivbscript:pt>
```
This input would be transformed incorrectly by `XSSRequestWrapper` into this:


```java
<script>alert(1)</script>
```
This is because XSSRequestWrapper replaces instances of `<script>` tags before it replaces instances of the `vbscript:` pattern. A correctly crafted input such as the one above would not only pass through, but will be changed from a merely incorrect html tag to a dangerous client side script.


## Recommended
Instead of relying on such an incomplete and porous defence, it is better to use well vetted libraries to accomplish XSS attack prevention. Examples of such libraries include the [OWASP Java Encoder](https://github.com/OWASP/owasp-java-encoder) . Additionally, many such sanitization measures could be taken at the client side, which, if paired with proper authentication of incoming requests can be very effective at stopping XSS attacks at the source.

