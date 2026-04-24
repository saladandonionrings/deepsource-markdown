# `@SpringBootApplication`/`@ComponentScan` annotations must not be used in the default package
**ID:** `JAVA-E1009` | **Link:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-E1009)

![Major](https://img.shields.io/badge/severity-major-orange)![Bug Risk](https://img.shields.io/badge/type-bug_risk-green)

Spring Boot uses annotations like [`@SpringBootApplication`](https://docs.spring.io/spring-boot/docs/current/api/org/springframework/boot/autoconfigure/SpringBootApplication.html), [`@ServletComponentScan`](https://docs.spring.io/spring-boot/docs/current/api/org/springframework/boot/web/servlet/ServletComponentScan.html) and [`@ComponentScan`](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/context/annotation/ComponentScan.html) to look for Spring beans to register as part of the application. `@ComponentScan` and `@ServletComponentScan` additionally allow one to set the package(s) to be scanned for beans (or servlets). If the type these annotations are marked with belongs to the default or root package, or if `@ComponentScan` is configured to search the root package, application startup may be slowed down to a crawl while the component scan completes.

In the worst case, the application may fail to start. This is because when the Spring Framework's own packages are scanned, predefined beans that are already registered will be encountered, triggering a `BeanDefinitionStoreException`.

`@SpringBootApplication` scans for beans within the package of the class it is annotated with.

`@ComponentScan` and `@ServletComponentScan` behave similarly to `@SpringBootApplication` by default, but also allow you to specify the package(s) to scan for beans through the `basePackages` and `basePackageClasses` values. They are aliased to the default value of the annotation, so you can also directly specify the package names or classes to use when scanning.

This issue is raised when these annotations are used on a class defined in the default package with empty values, or when the `@ComponentScan` or `@ServletComponentScan` annotations are used with one of the arguments being the base package.


## Bad Practice

```java
// in the root package

@SpringBootApplication
public class SBApplication {
    // ...
}
```
With `@ComponentScan`:


```java
@ComponentScan("")
public class SBApplication {
    // ...
}
```

## Recommended
Never use these annotations on classes defined in the default package. If absolutely necessary, make sure to only specify the packages that contain Spring beans.


```java
@ComponentScan("com.myapp.spring.beans")
public class SBApplication {
    // ...
}
```
