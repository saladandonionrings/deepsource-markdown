# Increment of volatile field is not atomic
**ID:** `JAVA-S0028` | **Lien:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-S0028)

![Major](https://img.shields.io/badge/severity-major-orange) ![Bug Risk](https://img.shields.io/badge/type-bug_risk-green)

An increment to a volatile field isn't atomic.

This code increments a volatile field. Increments of volatile fields aren't atomic. If more than one thread is incrementing the field at the same time, increments could be lost.


## Examples

## Bad Practice

```java
volatile int a;

// ...

a++;
```
The increment is essentially composed of 4 operations:

* Push `a` 's value onto the stack
* Copy the value once
* Increment the copied value
* Pop `a` 's value from the stack

Only the effects of step 1 and step 4 are visible to all threads. Because `a` is declared as being volatile, the effect of any access (read or write) to `a` will be visible across threads simultaneously. This does not mean that all of the operations performed by the imcrement will be executed atomically.

It is better to use an atomic integer class ( `java.util.concurrent.atomic.AtomicInteger` , for example) to avoid such issues.


## Recommended

```java
AtomicInteger a = new AtomicInteger(0);

// ...

int val = a.incrementAndGet();
```
