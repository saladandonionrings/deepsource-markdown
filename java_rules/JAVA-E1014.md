# Getter/setter names must be appropriate
**ID:** `JAVA-E1014` | **Lien:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-E1014)

![Minor](https://img.shields.io/badge/severity-minor-yellow) ![Anti-pattern](https://img.shields.io/badge/type-anti_pattern-purple)

Getter or setter methods must not perform operations other than reading or writing fields they are named after.

The main purpose of a getter is to allow easy access to private fields that the programmer wants to expose. Setters are, similarly, meant to allow users of a class to set the values of private fields.

When working with multiple fields that all need getters and/or setters, it is easy to make the mistake of just copy-pasting the same method, and forgetting to change the field that is returned/assigned, or forgetting to change the name of the method. Such mistakes can cost a great amount of time to find.

This issue is raised when:

* A getter does not read the field it is named after.
* A setter does not write to the field it is named after.


## Bad Practice

```java
private SomeClass something;
private String privateString;

// This method appears to get something, but returns privateString!
public String getSomething() {
    return privateString;
}

// This method seems to set privateString, but instead sets some other value!
public void setPrivateString(int value) {
    someInt = value;
}
```

## Recommended
Getters and setters work because of the assumed method contracts, that API consumers can get or set the value of the field represented by the method's name.

Always name getters and setters based on only the field they are reading or writing to.


```java
private String someString;
private int y;

public void setSomeString(String val) {
  someString = val;
}

public int getY() {
  return y;
}
```
A method that has the same name as a field, but performs some action other than getting or setting that field may benefit from renaming the method to avoid ambiguity for both API consumers as well as for future developers.


```java
// public class MediaPlayer
private ArrayList<Song> queue;

// This method looks like a getter but it instead does something else...
public void queue(Song s) {
    queue.add(s);
}
```
Consider renaming the method to be more appropriate in such cases.


```java
// The operation performed by this method is now very clear;
// we are enqueueing a new song into the queue.
public void enqueue(Song s) {
    // ...
}
```
