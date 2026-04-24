# Non-final static fields should not be public
**ID:** `JAVA-S1050` | **Link:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-S1050)

![Critical](https://img.shields.io/badge/severity-critical-red)![Security](https://img.shields.io/badge/type-security-red)

This code contains a public static field which is not final, or is mutable even when declared as final.

Consider making the field private, as it is possible that such a field could be manipulated to produce unintended results.


## Bad Practice
Here, the `NUM_RETRIES` field could be manipulated to perform a Denial of Service (DoS) attack when set to some very high number.


```java
class SomeClass {

    public static int NUM_RETRIES = 3;

}

// Elsewhere...

SomeClass someObj = ...;
SomeClass.NUM_RETRIES = Integer.MAX_VALUE; // This could make an application hang!
```

## Recommended
There are multiple ways to avoid this, and you must choose the best method as per your requirements.

**Make the field final**

If you do not need the field to be mutable, consider just making it final:


```java
public static final int NUM_RETRIES = 3;
```
**Make the field private**

If you require the field to be mutable, consider making the field private. If you also need to expose the field to API consumers, consider adding a static or instance getter method for the field:


```java
private static int NUM_RETRIES = 3;

// Static getter
public static final int getNumRetries() {
    return NUM_RETRIES;
}

// Instance getter, only usable when we have an instance of this class created.
public final int getNumRetries() {
    return NUM_RETRIES;
}
```
If you also need to be able to set the value, make sure to sanitize the assigned data. You could check if the retry value is within a maximum permissible limit (`MAX_NUM_RETRIES`) and if the assigned value is below 0 or above the maximum limit, clamp that value to within those limits.


```java
public static final void setNumRetries(int retries) {
    // clamp retries to within the range 0 to MAX_NUM_RETRIES.
    retries = (retries > MAX_NUM_RETRIES) ? MAX_NUM_RETRIES : ((retries < 0) ? 0 : retries);

    NUM_RETRIES = retries;
}
```
