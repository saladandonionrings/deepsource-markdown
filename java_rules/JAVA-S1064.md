# Paths in chained `antMatchers` invocations are not ordered by specificity
**ID:** `JAVA-S1064` | **Lien:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-S1064)

![Critical](https://img.shields.io/badge/severity-critical-red) ![Security](https://img.shields.io/badge/type-security-red)

URL patterns that appear in chained `antMatchers` calls should be ordered by specificity of the path that they match.

In Spring, it is common to configure URL access control by invoking `HttpSecurity.authorizeRequests()` and following that with a chain of `antMatchers` calls to specify the URL that needs to be restricted. The patterns that appear in `antMatchers` calls are considered in the order they appear.

For example, if one configures the path `user/**` to be accessible by everyone and a subsequent `antMatchers` call specifies that everything matching the pattern `user/admin` is accessible only to the admin, then the overall configuration will allow everyone to access `user/admin` .

More generally, if a less specific pattern appears before a more specific one and they happen to match the same paths, then it is possible, by mistake, to misconfigure access control so that the less specific matcher undoes the configurations set for more specific patterns.


## Bad Practice

```java
{
    http.authorizeRequests()
        .antMatchers("/resources/**").permitAll()
        .antMatchers("/resources/admin").hasRole("ADMIN");
 }
```

## Recommended
Consider ordering the `antMatchers` calls so that the more specific pattern appears early.


```java
{
    http.authorizeRequests()
        .antMatchers("/resources/admin").hasRole("ADMIN")
        .antMatchers("/resources/**").permitAll();
 }
```
