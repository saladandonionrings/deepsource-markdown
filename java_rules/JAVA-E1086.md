# Mutable data passed in to nonpublic field may be externally modifiable
**ID:** `JAVA-E1086` | **Link:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-E1086)

![Major](https://img.shields.io/badge/severity-major-orange)![Bug Risk](https://img.shields.io/badge/type-bug_risk-green)

This code seems to assign mutable data, such as an `ArrayList`, or a native Java array (like `int[]`) to a non-public field without first copying the data. This may lead to errors caused by inconsistent state if the passed in data is later modified from caller-side code.

When initializing an object, or invoking a setter for a field, it is common practice in Java to perform a [defensive copy](http://www.javapractices.com/topic/TopicAction.do?Id=15), where a deep copy of the input data is first made before it is used or assigned to anything else. This helps ensure that any change to the state of the object only happens by way of the object's methods, not by some external operation.


## Bad Practice
Consider this class declaration. It has one setter, `setIpAddrs`, which assigns the value of


```java
class SomeClass {
    private String[] ipAddrs;

    public void setIpAddrs(String[] addrs) {
        this.ipAddrs = addrs;
    }

    public void printAddrs() {
        for (int i = 0; i < ipAddrs.length; i++) {
            System.out.println(addrs[i]);
        }
    }
}
```
Now, consider this usage of the class:


```java
String[] addrs = new String[] { "192.168.10.23", "10.0.0.123" };

SomeClass instance = new SomeClass();

instance.setIpAddrs(addrs); // At this point, we have assigned addrs to ipAddrs.
```
If, at this point, we were to invoke `printAddrs()`, we would see this output:


```java
192.168.10.23
10.0.0.123
```
Now, consider what would happen if we change the value of one of `addrs`'s elements:


```java
addrs[1] = "Some random string";

instance.printAddrs();
```
This would print the following instead!


```java
192.168.10.23
Some random string
```

## Recommended
To avoid such problems, make a defensive copy of the data first.


```java
import java.util.Arrays;

class SomeClass {
    private String[] ipAddrs;

    public void setIpAddrs(String[] addrs) {
        // What we store is now a copy of the original array.
        this.ipAddrs = Arrays.copyOf(addrs, addrs.length);
    }
}
```
Now, even if the original array were modified, the array stored inside `SomeClass` itself would be preserved.

