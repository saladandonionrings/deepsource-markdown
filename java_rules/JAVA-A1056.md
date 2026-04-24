# Audit: Thread.sleep() call may be at risk of DoS attack
**ID:** `JAVA-A1056` | **Lien:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-A1056)

![Critical](https://img.shields.io/badge/severity-critical-red) ![Security](https://img.shields.io/badge/type-security-red)

Calls to `Thread.sleep()` should not accept untrusted input, as an attacker could send a very long sleep value, causing a Denial of Service (DoS) attack.

The argument to `sleep()` (of type `long` ) dictates how long a thread will be suspended for.

If this argument is set to a very high value (and that value could be in the [low quintillions](https://stackoverflow.com/questions/6003492/how-big-can-a-64bit-signed-integer-be) ), you could end up with a thread that tries to sleep for about 28 billion years.

Such vulnerabilities should be avoided.


## Bad Practice
This example uses a web servlet:


```java
Long time = Long.parseLong(req.getParameter("sleeptime"));

if (time == null) // handle null value

// ...

// time is controlled by the sleeptime request parameter now!
Thread.sleep(time);
```

## Recommended
Cap the maximum sleep time to some reasonable value:


```java
long TIMEOUT_MAX = 2000l;

Long time = Long.parseLong(req.getParameter("sleeptime"));

// cap the sleep time to a specific maximum value.
if (time > TIMEOUT_MAX) time = TIMEOUT_MAX;

// ...

Thread.sleep(time);
```
