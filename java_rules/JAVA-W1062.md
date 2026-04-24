# Wrong thread for swing method invocation
**ID:** `JAVA-W1062` | **Link:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-W1062)

![Major](https://img.shields.io/badge/severity-major-orange)![Bug Risk](https://img.shields.io/badge/type-bug_risk-green)

Methods `show()`, `setVisible()`, and `pack()` must not be invoked on the main thread.

With each invocation, these methods create the peer for the associated `JFrame`. Creation of the peer involves creation of the event dispatch thread. At this point, the event dispatch thread could be in the middle of notifying listeners while `show()`, `setVisible()`, or `pack()` is still executing. As a result, we could have two threads going through the Swing component-based GUI at the same time, which is a serious issue that might lead to deadlocks or other multithreading bugs.


## Bad Practice

```java
public void method() {
    // These are problematic.
    frame.show();
    frame.setVisible(true);
    frame.pack();
}
```

## Recommended
Consider calling these methods on the event dispatch thread.


```java
public void method() {
    SwingUtilities.invokeLater(new Runnable() {
        @Override
        public void run() {
            frame.show();
            frame.setVisible(true);
            frame.pack();
        }
    }
}
```
