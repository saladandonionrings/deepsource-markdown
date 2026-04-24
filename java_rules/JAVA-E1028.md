# `setUp` and `tearDown` methods must be properly annotated
**ID:** `JAVA-E1028` | **Lien:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-E1028)

![Major](https://img.shields.io/badge/severity-major-orange) ![Bug Risk](https://img.shields.io/badge/type-bug_risk-green)

JUnit 3 introduced the `setUp` and `tearDown` methods as part of the `TestCase` API. These methods allow tests to perform operations before and after a test has run.

However, they cannot be directly used in JUnit 4 and 5; they must be marked with specific annotations to preserve their functionality.

When migrating from JUnit3 to JUnit4, the [ `@Before` ](https://junit.org/junit4/javadoc/latest/org/junit/Before.html) and [ `@After` ](https://junit.org/junit4/javadoc/latest/org/junit/After.html) annotations, or the [ `@BeforeClass` ](https://junit.org/junit4/javadoc/latest/org/junit/BeforeClass.html) and [ `@AfterClass` ](https://junit.org/junit4/javadoc/latest/org/junit/AfterClass.html) annotations must be used.

When migrating to JUnit5, the [ `@BeforeEach` ](https://junit.org/junit5/docs/current/api/org.junit.jupiter.api/org/junit/jupiter/api/BeforeEach.html) and [ `@AfterEach` ](https://junit.org/junit5/docs/current/api/org.junit.jupiter.api/org/junit/jupiter/api/AfterEach.html) annotations, or the [ `@BeforeAll` ](https://junit.org/junit5/docs/current/api/org.junit.jupiter.api/org/junit/jupiter/api/BeforeAll.html) and [ `@AfterAll` ](https://junit.org/junit5/docs/current/api/org.junit.jupiter.api/org/junit/jupiter/api/AfterAll.html) annotations must be used.


## Bad Practice

```java
public void setUp() { // Needs an annotation now...
    // ...
}

// Needs to be annotated.
public void tearDown() {
    // ...
}
```

## Recommended
For JUnit4:


```java
@Before
public void setUp() {
    // ...
}

@After
public void tearDown() {
    // ...
}
```
For JUnit5:


```java
@BeforeEach
public void setUp() {
    // ...
}
```
