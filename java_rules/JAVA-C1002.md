# Wrong argument order in test assertions
**ID:** `JAVA-C1002` | **Link:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-C1002)

![Major](https://img.shields.io/badge/severity-major-orange)![Style](https://img.shields.io/badge/type-style-blue)

Argument ordering conventions must be followed for assertions in their respectice libraries.

JUnit assertion methods such as `assertEquals` expect the first argument to be the expected value and second to be the actual value. On the other hand, AssertJ works in reverse. Methods such as `assertThat()` must be passed the actual value first, and the expected value is passed in the subsequent chained call to `isEqualTo()`, `isGreaterThan()` or a similar combinator method.

As far as test execution goes, the order of arguments does not matter; test results will not vary just because arguments are passed in the wrong order. But, changing the order can cause problems when reading test reports or test output (IntelliJ allows you to see a diff between expected and actual results, for example) leading to weird behavior. In general, it is a good practice to follow the conventions set by a particular test framework.


## Bad Practice
Avoid passing arguments to test methods in arbitrary order.


```java
class MyTest {
    @Test
    void test() {
        org.junit.Assert.assertEquals(value, 10);
        org.assertj.core.api.Assertions.assertThat(10).isEqualTo(value);
    }
}
```

## Recommended
Instead, consider following the conventions set by the test framework that you are using.


```java
class MyTest {
    @Test
    void test() {
        org.junit.Assert.assertEquals(10, value);
        org.assertj.core.api.Assertions.assertThat(value).isEqualTo(10);
    }
}
```
