# Synchronizing on a mutable reference may lead to unexpected behavior
**ID:** `JAVA-S0153` | **Lien:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-S0153)

![Major](https://img.shields.io/badge/severity-major-orange) ![Bug Risk](https://img.shields.io/badge/type-bug_risk-green)

This method is attempting to synchronize on a field whose value may change. This is very dangerous and may easily lead to difficult to diagnose bugs.

Consider the case of a mutable private field intended to be synchronized on:


```java
private Long myNtfSeqNbrCounter = new Long(0);
```
And here is a method which attempts to acquire a lock on this field to provide mutual exclusion:


```java
private Long getNotificationSequenceNumber() {
     Long result = null;
     synchronized(myNtfSeqNbrCounter) {
         result = new Long(myNtfSeqNbrCounter.longValue() + 1); 
         myNtfSeqNbrCounter = new Long(result.longValue()); // !!! myNtfSeqNbrCounter no longer points to the value we are locking on.
     }
     return result;
}
```
In the above code, `myNtfSeqNbrCounter` points to a new object each time `getNotificationSequenceNumber` is called. This means that any threads that called this function in the past may not have been synchronizing on the same object. The likelihood of 2 or more threads simultaneously changing the value of `myNtfSeqNbrCounter` is very high.

Ensure that any value that will be locked on is marked as `private final` .

