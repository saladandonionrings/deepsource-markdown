# Detected synchronization on string literal
**ID:** `JAVA-E1081` | **Link:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-E1081)

![Major](https://img.shields.io/badge/severity-major-orange)![Bug Risk](https://img.shields.io/badge/type-bug_risk-green)

Avoid synchronizing on string literals.

The same object internally represents strings having the same content; JVM does that to reduce memory requirements.

Synchronizing on string literals has the risk of allowing deadlocks to occur. For example, if two different pieces of code synchronize on the same string object, it may be possible to cause a deadlock if they run parallelly.


## Bad Practice

```java
class MyClass {
    private static final String MONITOR = "common-string";

    public void doThing() {
        synchronize(MONITOR) {
            // ...code that is supposed to run in parallel to the code in `ThirdPartyClass::doAnotherThing`.
        }
    }
}

// Someplace else.
class ThirdPartyClass {
     // Note that the content of this string is same as of `MONITOR`'s.
    private static final String THIRD_PARTY_MONITOR = "common-string";

    public void doAnotherThing() {
        synchronize(THIRD_PARTY_MONITOR) {
            // ...code that is supposed to run in parallel to the code in `MyClass::doThing`.
        }
    }
}
```
The code inside the methods `MyClass::doThing()` and `ThirdPartyClass::doAnotherThing()` won't run in parallel (even though they are supposed to) because we are essentially synchronizing on the same string object. Depending on the functionalities of these two methods, we might end up causing a deadlock or some other synchronization bug here.


## Recommended

```java
class MyClass {
    private static final Object MY_LOCK = new Object();

    public void doThing() {
        synchronize(MY_LOCK) {
            // ...code that is supposed to run in parallel to the code in `ThirdPartyClass::doAnotherThing`.
        }
    }
}

// Someplace else.
class ThirdPartyClass {
    private static final Object THIRD_PARTY_LOCK = new Object();

    public void doAnotherThing() {
        synchronize(THIRD_PARTY_LOCK) {
            // ...code that is supposed to run in parallel to the code in `MyClass::doThing`.
        }
    }
}
```
