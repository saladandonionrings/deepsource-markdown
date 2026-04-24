# Anonymous classes should not contain unused non-overridden methods
**ID:** `JAVA-W1039` | **Lien:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-W1039)

![Major](https://img.shields.io/badge/severity-major-orange) ![Anti-pattern](https://img.shields.io/badge/type-anti_pattern-purple)

A non-overridden method is defined within an anonymous class, which is not used within that class at all.

Such a method cannot be called outside of the anonymous class in most cases, meaning it is a useless declaration.

Because anonymous classes have no names, there is no way at runtime for code outside an anonymous class to resolve any methods declared within them. The only way for an anonymous class to be interacted with by any other object is to override methods that exist in the parent class/interface(s) of the anonymous class.

Do note that there are still a number of ways to access such methods, through reflection, and other [tricks](https://stackoverflow.com/questions/10800678/can-i-access-new-methods-in-anonymous-inner-class-with-some-syntax) .


## Bad Practice

```java
Object anon = new Runnable() {
    @Override
    public void run() {
        // ...
    }

    // This method my be public, but it can't be called from outside.
    public void thing() {
        System.out.println("thing");
    }
}

anon.thing(); // This won't compile!
```

## Recommended
If you wish to create new methods to be called by external code, consider creating a proper class, or an inner (static) class that can have new methods.


```java
class CustomRunnable extends Runnable {

    @Override
    public void run() {
        // ...
    }

    public void thing() {
        System.out.println("thing");
    }

}

CustomRunnable cr = new CustomRunnable();

cr.thing(); // this works!
```
