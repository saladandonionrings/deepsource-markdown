# Injected fields should not be assigned in injection constructors
**ID:** `JAVA-W1079` | **Lien:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-W1079)

![Critical](https://img.shields.io/badge/severity-critical-red) ![Anti-pattern](https://img.shields.io/badge/type-anti_pattern-purple)

Avoid modifying fields that are already annotated with `@Inject` inside an `@Inject` annotated constructor.


## Bad Practice
Here, `thing` is marked as injected. However, `Example` 's constructor is also marked with `@Inject` . This is redundant and could cause issues when initializing `thing` through both constructor and field injection.


```java
class Example {
    @Inject
    String thing;

    @Inject
    public Example(String s) {
        thing = s; // thing is already supposed to be injected!
    }
}
```

## Recommended
Only assign fields that are not marked as `@Inject` in the constructor.


```java
class Example {
    @Inject
    String thang;

    String thing;

    @Inject
    public Example(String s) {
        thing = s;
    }
}
```
