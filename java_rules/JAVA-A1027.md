# Audit: Setting bean properties with unsanitized input may be a security risk
**ID:** `JAVA-A1027` | **Lien:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-A1027)

![Critical](https://img.shields.io/badge/severity-critical-red) ![Security](https://img.shields.io/badge/type-security-red)

Be careful when setting bean properties using external data.

Java beans are classes that implement getters and setters for their fields in conformance with the [JavaBeans](https://www.oracle.com/java/technologies/javase/javabeans-spec.html) specification. Libraries such as Apache's [Commons BeanUtils](https://commons.apache.org/proper/commons-beanutils/) use reflection to access fields and set or retrieve their values.

Managing data through beans is versatile, but can also reduce security. For example, a particular version of Apache's BeanUtils used within the [Struts](https://struts.apache.org/) web framework was susceptible to certain [class loader related attacks](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2014-0114) . This attack consisted of accessing the `class` property of a bean, through which the [ `ClassLoader` ](https://www.baeldung.com/java-classloaders) for that bean could be accessed. Obtaining a reference to a `ClassLoader` can allow loading an attacker-defined class into the application, achieving arbitrary code execution.

This issue is raised if methods such as [ `BeanUtils.populate()` ](https://commons.apache.org/proper/commons-beanutils/javadocs/v1.9.4/apidocs/org/apache/commons/beanutils/BeanUtilsBean.html#populate-java.lang.Object-java.util.Map-) or Spring's [ `BeanWrapper.setPropertyValue()` ](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/beans/BeanWrapper.html) are called with possibly unsanitized input.


## Bad Practice

```java
class UserDataBean { /*...*/  }

@Override
void method() {
    
    HashMap map = new HashMap();
    Map<String, String[]> params = request.getParameterMap();
    UserDataBean bean = new UserDataBean();
    
    
    BeanUtils.populate(bean, params); // Insecure.
}
```

## Recommended
Sanitize any data that will pass into a JavaBean instance. How you do so will be very specific to your own requirements, but here are a few suggestions:

* Use a whitelist to verify that any data that has a distinct set of values cannot be tampered with.


```java
Map<String, String[]> finalParams = new HashMap<>();
for (Map.Entry<String, String[]> entry : params.entrySet()) {
    if ( !allowedKeys.contains(entry.getKey())) continue; // Filter out unnecessary keys.
        
    finalParams.put(entry.getKey(), entry.getValue());
}

BeanUtils.populate(bean, finalParams);
```
* Data such as request parameters, headers and cookies should be handled carefully, as they are the largest attack surfaces.
* Use a data sanitization library like OWASP's [ESAPI](https://owasp.org/www-project-enterprise-security-api/) to reduce the amount of work you need to do.

If any of the data in the request is used in the response, care must be taken to avoid injecting malicious data from the request into the response, as this could lead to a server-side injection attack.

