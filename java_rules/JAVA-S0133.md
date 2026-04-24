# Reference to externally mutable object stored as internal state
**ID:** `JAVA-S0133` | **Lien:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-S0133)

![Critical](https://img.shields.io/badge/severity-critical-red) ![Security](https://img.shields.io/badge/type-security-red)

This code stores a reference to an externally mutable object as part of the internal state of the object. This can allow an injection attack to occur.

This issue is an inversion of `JAVA-S0132` , which is triggered by the internal state being exposed by allowing external access to a mutable field.

It may be possible to modify internal state of the object from any code that uses it through a mutable field which can be accessed publicly. This is common in cases where Java arrays ( `Object[]` ) are passed around directly.

Values such as arrays, mutable data structures and any user defined mutable classes can be dangerous when used without proper precautions.

If any of the following situations apply, you may want to structure this code differently:

* The encapsulating class will modify data which is provided from outside
* Unchecked changes to the referenced field would compromise security or other important properties


## Bad Practice

```java
class Insecure {

    private String[] ipAddresses;

    public Insecure(String[] ipAddresses) {
        this.ipAddresses = ipAddresses;
    }

    // ...

    public void doSomething() {
        // Does something with the ip addresses...
    }
}

// In calling code:

String[] addrs = getIpAddressesSomehow(); // we get a list of data...
Insecure insecure = new Insecure(addrs);

// We still have access to addrs, which references the same data passed to Insecure's constructor.
// We can therefore influence the operation of Insecure through this reference.
// assigning to addrs here changes the value of ipAddresses within insecure.
addrs[1] = "192.168.1.1"; // This could be a malicious ip address...
```

## Recommended
When a value which supports mutation is to be assigned, create a copy of it and assign the copy instead of the original value. There are various ways to accomplish this, depending on the type of the object in question.

Arrays can be copied using the ststic `Arrays.copyOf` method.


```java
public Insecure(String[] ipAddresses) {
    String[] ipAddressesCopy = Arrays.copyOf(ipAddresses);
    this.ipAddresses = ipAddressesCopy;
}
```
If you are copying objects, you have a choice between using a copy constructor if the class provides it, or, if the class implements `Cloneable` , the class's clone method:


```java
// Using the copy constructor
public Secure(MutableData mData) {
    MutableData data = new MutableData(mData);
    this.mData = data;
}

// Using the clone method, if the object implements Cloneable.
public  Secure(CloneableData cData) {
    this.cData = cData.clone();
}
```
If neither of these methods are available, or the values are provided by code you do not control, consider making use of the Java serialization API to make the copy. This is slower and will only work if the class in question implements `java.io.Serializable` however.

