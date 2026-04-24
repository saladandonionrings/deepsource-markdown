# Getter and setter method synchronization does not match
**ID:** `JAVA-E1074` | **Link:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-E1074)

![Major](https://img.shields.io/badge/severity-major-orange)![Bug Risk](https://img.shields.io/badge/type-bug_risk-green)

This class contains get and set methods where one is synchronized but the other is not. Such code may allow race conditions to occur while reading or writing the associated field.

This may allow race conditions to occur while reading the object, causing missed updates and other bad behavior. Both getters and setters should be synchronized.

This checker will report cases where accessors have mismatched synchronization methods, or one accessor has no synchronization while another does.


## Bad Practice
In this example, `name` has a synchronized method getter, and a non-synchronized setter.


```java
private String name;

public synchronized String getName() {
    return this.name;
}

// Not synchronized!
public void setName(String newName) {
    this.name = newName;
}
```
Here, `values` has a synchronized method getter (the getter synchronizes on `this`) and a setter with a synchronized block (the setter synchronizes on `this.LOCK`).


```java
private final Object LOCK = new Object();
private HashMap<Integer,  Integer> values;

public synchronized Map<Integer, Integer> getValues() {
    return this.values;
}

// The setter synchronizes on a different object than the getter!
public void setValues(Map<Integer, Integer> newValues) {
    synchronized(LOCK) {
        this.values = newValues;
    }
}
```
Here, `items` has an unsynchornized getter, and its setter synchronizes on `LOCK`.


```java
private final Object LOCK = new Object();
private List<Item> items;

public List<Item> getItems() {
    return this.items;
}

// The setter synchronizes on LOCK, but the getter does not.
public void setValues(List<Item> newItems) {
    synchronized(LOCK) {
        this.items = newItems;
    }
}
```

## Recommended
Either synchronize all accessor methods with the same mechanism, or do not attempt to synchronize any accessor method at all.

The example below uses `synchronized` methods, but this also applies to `synchronized` blocks.


```java
private String name;

public synchronized String getName() {
    return this.name;
}

public synchronized void setName(String newName) {
    this.name = newName;
}
```
