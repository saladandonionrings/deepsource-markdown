# Non-thread-safe date/time fields should not be public and static
**ID:** `JAVA-E1024` | **Lien:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-E1024)

![Major](https://img.shields.io/badge/severity-major-orange) ![Bug Risk](https://img.shields.io/badge/type-bug_risk-green)

Avoid storing non-thread-safe `java.util` date/time API classes in public static fields.

These classes are not designed to be used directly over multiple threads and issues such as race conditions or spurious crashes may occur.

This issue will be reported if a `public static` field is found having any of the following types:

* [ `java.util.Calendar` ](https://docs.oracle.com/en/java/javase/17/docs/api/java.base/java/util/Calendar.html)
* [ `java.text.SimpleDateFormat` ](https://docs.oracle.com/en/java/javase/17/docs/api/java.base/java/text/SimpleDateFormat.html#synchronization)

`Calendar` is particularly insidious in this, as its documentation makes no mention of its lack of thread safety.


## Bad Practice
Consider the example below, which uses `Calendar` (the same principle applies to `SimpleDateFormat` as well). Because `CAL_INSTANCE` is `public` , it can be accessed by any external code.


```java
public static final Calendar CAL_INSTANCE = ...;
```
Modifying (or even accessing fields of) a `Calendar` instance will mutate it, and such mutations can cause unintended state changes on other threads.


## Recommended
If a single global `Calendar` instance is absolutely required, make sure to keep the field `private` , and predefine all possible actions that will be performed with the object as synchronized methods. This reduces the chances of race conditions occurring due to unsynchronized usage of the specific calendar instance.


```java
private static final Object LOCK = new Object();
private static final Calendar cal = ...;

public Date getDateAfter(int days) {
    synchronized(LOCK) {
        cal.add(Calendar.DAY, days);
        Date date = cal.getTime();
        cal.add(Calendar.DAY, -days);
        return date;
    }
}
```
If each thread requires a persistent instance, consider wrapping the field in a `ThreadLocal` instead. This will allow for keeping the field `public` and `static` while still preserving thread safety.


```java
public static ThreadLocal<Calendar> = ThreadLocal.withInitial(() -> Calendar.getInstance());
```
If a global instance is not required, just use an instance value, or create a new instance on demand.

**Alternatives**

If possible, consider using a better date/time API such as [ `java.time` ](https://www.oracle.com/technical-resources/articles/java/jf14-date-time.html) (For Java versions 8 and above) or [Joda-time](https://www.joda.org/joda-time/) (For Java versions 7 and below), which do not have such thread safety issues.

