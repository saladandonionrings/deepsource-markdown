# Avoid assertions within `Runnable`s
**ID:** `JAVA-W1096` | **Lien:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-W1096)

![Major](https://img.shields.io/badge/severity-major-orange) ![Anti-pattern](https://img.shields.io/badge/type-anti_pattern-purple)

A `Runnable` is generally used to execute code on multiple threads.

If an assertion (like `assertEquals()` ) were to execute and fail within a separate thread, there would be no way for the test framework to recognize the failed assertion due to how testing works in Java.

If you are using a test framework that can properly handle assertions across threads, you can ignore this issue with a `// skipcq: JAVA-W1096` comment on the same line as the reported assertion.

When you run a JUnit test, the JVM doesn't immediately start running the code in your test files. There's a lot of work that goes on to get you the pretty assertions and test orchestration functionality within tests.

To elaborate, the code that checks and reports failures sort of looks like this:


```java
try {
    runTests()
} catch (AssertionError e) {
    // Tell user that tests failed.
}
```
The next time you're debugging a test, have a look at the things that go on above your test code in the call stack!

When you call an `assert*` method and it fails, an `AssertionError` is thrown to signal that some test criteria failed, and is caught and handled by the try block above. That `try` block also only runs on the main thread, not on any other ones you start within tests.

So if you try to assert something in a new thread, the resulting `AssertionError` will only kill that particular thread, not fail tests (directly).

Even if tests do fail because a thread died, it would only be because the preconditions of a subsequent assertion on the main thread failed, not because of the uncaught one.


## Bad Practice
Avoid assertions within any code that will run on a different thread.


```java
Runnable runnable = () -> {
    Assertions.assertEquals(2, Thread.activeCount()); // will never be caught in the main thread
};
```

## Recommended
You can instead create unit tests specific to the multithreaded code, and run them in isolation.

You can then also add integration tests that verify that the output of the multithreaded code interacting with the main thread is correct.

This would allow for easier testing, and would ensure proper test coverage as well.

