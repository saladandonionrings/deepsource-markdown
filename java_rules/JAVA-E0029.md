# Unsafe usage of `getResource`
**ID:** `JAVA-E0029` | **Lien:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-E0029)

![Major](https://img.shields.io/badge/severity-major-orange) ![Bug Risk](https://img.shields.io/badge/type-bug_risk-green)

Usage of `getResource` may be unsafe if this class is extended.

Calling `this.getClass().getResource(...)` with a non-absolute path could have unexpected results if this class is extended by a class in another package. Resources defined in one package may not be available in another. In addition, there is a possibility of different packages having different resources under the same name, which may lead to unexpected bugs from accessing the wrong resource.


## Bad Practice

```java
getClass().getResource("someResource");
```
Consider how `getClass` works. `getClass` effectively returns the final runtime type of an object:


```java
Number n1 = new Float(3.2f);
n1.getClass(); // Float.class

n1 = new Integer(3);
n1.getClass(); // Integer.class
```
Note that it returns the referenced object's type, not the reference's type. Now consider the case of two maven modules both of which declare classes under different packages within the same group:


```java
Project
+- Project-subproject1
|  +- src/main/java      - com.example.project.subproject1.Class1
|  \- src/main/resources - com/example/project/subproject1/a.png
\- Project-subproject2
   +- src/main/java      - com.example.project.subproject2.Class2
```
Within `Class1.java` :


```java
// ...

getClass().getResource("a.png");

// ...
```
If `Class2` inherits from `Class1` , the `getClass` call would return `Class2.class` , not `Class1.class` . `Project-subproject1` defines a resource `a.png` within its resource directory, which is accessible with a relative path from `Class1.getClass().getResource(...)` . If the same code runs in `Class2` 's context, an `IOException` may occur because the resource is not declared relative to `Class2` . Unless the resources are referred to with absolute paths like `/com/example/subproject1/a.png` , each class can only access the resource declared under its own respective package.

Such code is likely to break in production (if it hasn't already broken in testing).


## Recommended
Access the resource through the static class object, or using an absolute path instead:


```java
Class1.class.getResourceAsStream("a.png");

// OR:

getClass().getResource("/com/example/subProject1/a.png");
```
