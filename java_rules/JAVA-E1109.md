# Calls to assertion chain methods should be terminated with an assertion
**ID:** `JAVA-E1109` | **Link:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-E1109)

![Major](https://img.shields.io/badge/severity-major-orange)![Bug Risk](https://img.shields.io/badge/type-bug_risk-green)

Always terminate calls to `assertThat()` or `verify()` with a relevant assertion call, such as `equals()`, or similar.

Assertion frameworks such as Assert4J and Truth have fluent APIs, where assertions are represented as chained method calls.


```java
assertThat(something).isEqualTo("somethingElse");
```
While the fluent style is very convenient and can improve the developer experience, it is also easy to make a mistake by starting an assertion chain, but forgetting to end it.

This would make the assertion useless.

What's worse is that such an incomplete assertion will not fail your tests, lulling you into a false sense of security.

This issue is reported for any test frameworks such as Assert4J or Mockito which support fluent assertions through methods such as `assertThat` or `verify`.


## Bad Practice

```java
assertThat(someValue); // This assertion is incomplete.

// for mockito
verify(someMethod);
```

## Recommended

```java
assertThat(someValue).isGreaterThan(expectedValue);
```

## Exceptions
This issue will not be reported if the call is used within an expression, such as by returning it or passing it to a different function.

