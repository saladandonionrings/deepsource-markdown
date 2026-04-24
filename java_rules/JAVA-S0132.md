# Reference to mutable object which is returned may expose internal representation of data
**ID:** `JAVA-S0132` | **Lien:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-S0132)

![Critical](https://img.shields.io/badge/severity-critical-red) ![Security](https://img.shields.io/badge/type-security-red)

Returning a reference to a mutable value stored in any of an object's fields exposes the internal state of the object. This could lead to an injection vulnerability.

It may be possible to modify internal state of the object from any code that uses it through a mutable field which can be accessed publicly. This issue is common in cases where Java arrays ( `Object[]` ) are passed around directly.

If any of the following situations apply, you may want to structure this code differently:

* Instances of the referenced field are accessed by untrusted code
* Unchecked changes to the referenced field would compromise security or other important properties

Returning a new copy of the field is a better approach in many situations. If the field's class type is controlled by you, consider implementing the `Cloneable` interface for that field, or create a copy constructor for it.


## Bad Practice
Consider the following class, with a private `String[]` value, `ipAddresses` :


```java
class Insecure {

    private String[] ipAddresses;

    public Insecure() {
        ipAddresses = new String[20];
    }

    public String[] getIpAddresses() {
        return this.ipAddresses;
    }

    // ...

    public void doSomething() {
        // Does something with the ip addresses...
    }
}
```
Now, consider this calling code, which performs some operations on an instance of the class:


```java
Insecure insecure = new Insecure();

String[] addrs = insecure.getIpAddresses();
```
When we assign to `addrs` , a local variable, we see something surprising:


```java
// assigning to addrs here changes the value of ipAddresses within insecure.
addrs[1] = "192.168.1.1"; // This could be a malicious ip address...
addrs[1].equals(insecure.getIpAddresses()[1]); // This will return true, indicating that the values within insecure also were modified.
```

## Recommended
When returning a value which supports mutation, create a copy of it and return the copy instead of the original value. There are various ways to accomplish this, depending on the type of the object being returned.

Arrays can be copied using the static `Arrays.copyOf` method.


```java
public String[] getIpAddresses() {
    return Arrays.copyOf(this.ipAddresses);
}
```
If you are copying objects, you have a choice between using a copy contructor if the class provides it, or if the class implements `Cloneable` , the class's `clone` method:


```java
// Using the copy constructor
public CopyableObj getIpAddresses() {
    return new CopyableObj(this.copyValue);
}

// Using the clone method, if the object implements Cloneable.
public CloneableObj getIpAddresses() {
    return this.cloneValue.clone();
}
```
If neither of these methods are available, or the values are provided by code you do not control, consider making use of the Java serialization API to make the copy. This is slower and will only work if the class in question implements `java.io.Serializable` however.

