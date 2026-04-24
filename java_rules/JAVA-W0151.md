# `synchronized` block is empty
**ID:** `JAVA-W0151` | **Lien:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-W0151)

![Major](https://img.shields.io/badge/severity-major-orange) ![Anti-pattern](https://img.shields.io/badge/type-anti_pattern-purple)

The code contains an empty synchronized block. This could confuse readers of this code later.

An empty `synchronized` block seems a bit superfluous at first:


```java
synchronized(...) {}
```
This construct can still be useful though; it can work as a simple write-before-read memory barrier without using volatile variables due to the synchronization guarantees that `synchronized` blocks provide. However, this way of using `synchronized` blocks may not be as easily understood as a more explicit mechanism such as a `Semaphore` .

If this was intended, make sure to document the usage, or rewrite it to make things clearer using abstractions such as `Semaphore` s.


## Bad Practice

```java
int variable;
final Object o = new Object();

// ...

new Thread( () ->  // A
{
    // This will become visible to B after the synchronized block.
    variable = 9;
    synchronized( o ) {}
    
    // ... 
}).start();

new Thread( () ->  // B
{
    // ...
    do {
        synchronized( o ) {}
        // This will pick up the change made in A at some point.
    } while (variable != 9);
    // ...
}).start();
```

## Recommended
The following code with a `java.util.conncurrent.Semaphore` could be more clear:


```java
int variable;
final Semaphore sem = new Semaphore(0);

// ...


new Thread(() -> { // B
    try {
        // Will wait until A has updated variable's value.
        sem.acquire();
    } catch (Exception e) {
        e.printStackTrace();
    }
    
    if (var == 9) { ... }
}).start();


new Thread(() -> { // A
    variable = 9;
    // This will be visible within B after sem is released.
    sem.release();
}).start();
```
