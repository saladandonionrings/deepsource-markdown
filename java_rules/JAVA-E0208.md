# Constructor of non-final class starts a thread
**ID:** `JAVA-E0208` | **Lien:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-E0208)

![Major](https://img.shields.io/badge/severity-major-orange) ![Bug Risk](https://img.shields.io/badge/type-bug_risk-green)

The constructor starts a thread. This is likely to go wrong if the class is ever extended/subclassed, since the thread will be started before the subclass constructor is executed and will probably cause unexpected behavior.

The Java language specification section [12.5](https://docs.oracle.com/javase/specs/jls/se11/html/jls-12.html#jls-12.5) states that if, within any class constructor, another constructor (be it of the superclass or another constructor within the same class) is not explicitly invoked before any other statement, Java will implicitly invoke the super constructor before executing other statements in the body.

Because of this behavior, the thread is guaranteed to start before any subclass constructor code is executed. If the subclass modifies state touched by the thread while the thread is still running, there is a chance of data becoming corrupted. In the best case, this would lead to a crash with an appropriate exception. In the worst case, the JVM may not realize that data may be corrupted.

If this class is not meant to be inherited from, declare it as final. Otherwise, consider refactoring the code so the thread is started after the constructor runs, to ensure deterministic behavior.


## Bad Practice

```java
class SomeClass {

    // This is set by the thread...
    String res = null;
    private Thread t = null;


    public SomeClass() {

        t = new Thread(new Runnable() {
            @Override
            public void run() {
                // ... lengthy op ...

                // res is set here...
                res = "abc";
            }
        });

        t.start();

    }
}


class SomeOther extends SomeClass {
    public SomeOther() {
        // The super constructor is implicitly called here..

        // ...


        // We write to res here..
        // Are we overwriting the value assigned by the thread?
        // Or will the thread overwrite this value?
        res = "Something";
    }
}
```
