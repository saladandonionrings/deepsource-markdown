# Spring component introduces unmanaged state
**ID:** `JAVA-S1060` | **Link:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-S1060)

![Critical](https://img.shields.io/badge/severity-critical-red)![Security](https://img.shields.io/badge/type-security-red)

Spring components should not introduced unmanaged state variables (fields not managed by Spring).

Spring components such as `@Component`, `@Controller`, `@Service`, and `@Repository` are supposed to be singletons by default. This means that no more than one instance of such classes must exist in an application. Furthermore, the state of these classes is managed by the Spring container.

Non-injected properties in such classes could indicate an attempt to manage state. This introduces the risk of exposing data to clients that shouldn't have access to such data. For example, one might accidentally allow `User1` to access `User2`'s session if such patterns are followed throughout the source code.


## Bad Practice

```java
@Component
public class MyComponent {
    private Service someService;
}
```

## Recommended
Consider injecting these fields manually.


```java
@Component
public class MyComponent {
    @Autowired
    private final Service someService;
}
```
Alternatively, use constructor injection to inject dependencies.


```java
@Component
public class MyComponent {
    private final Service someService;

    @Autowired
    public MyComponent(Service someService) {
        this.someService = someService;
    }
}
```
