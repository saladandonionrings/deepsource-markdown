# `@RequestMapping` must restrict the allowed HTTP methods
**ID:** `JAVA-S1065` | **Lien:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-S1065)

![Critical](https://img.shields.io/badge/severity-critical-red) ![Security](https://img.shields.io/badge/type-security-red)

Request handlers annotated with `@RequstMapping` must limit the allowed HTTP methods.

Request handlers annotated with `@RequstMapping` are mapped to all HTTP request methods. For security reasons, CSRF protection is disabled for `GET` , `HEAD` , `TRACE` , and `OPTIONS` requests by default. If a request handler annotated with `@RequestMapping` happens to modify application state and the allowed HTTP method is not narrowed down to `POST` , `PUT` , `DELETE` , and/or `PATCH` , such misconfigurations can make your application susceptible to CSRF attacks.


## Bad Practice

```java
@RequestMapping("/path")
public void saveToDB() {
    // ...proceed to modify application state
}
```

## Recommended
When using `@RequestMapping` , make sure to always limit which HTTP methods are allowed.


```java
@RequestMapping(value = "/path", method = RequestMethod.POST)
 public void saveToDB() {
    // ...proceed to modify application state
}
```
