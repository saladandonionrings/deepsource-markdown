# Class extends Servlet class and uses instance variables
**ID:** `JAVA-E0370` | **Link:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-E0370)

![Critical](https://img.shields.io/badge/severity-critical-red)![Bug Risk](https://img.shields.io/badge/type-bug_risk-green)

This class extends from a Servlet class and uses an instance member variable. Since only one instance of a Servlet class is created by the J2EE framework and is used in a multithreaded way, there is only ever one copy of any field within the servlet. It is always better to mark global singletons as such, and the best way to do so is to use a static final field.

Consider only using method local variables, or use static fields with proper synchronization.


## Bad Practice

```java
@WebServlet(value="/helloWorld", name="helloWorldServlet")
public class HelloWorldServlet extends HttpServlet {

    private HashMap<String, Integer> uniqueVisits = new HashMap<>();

    // ...
}
```

## Recommended
Using a final static field:


```java
@WebServlet(value="/helloWorld", name="helloWorldServlet")
public class HelloWorldServlet extends HttpServlet {
    private static final HashMap<String, Integer> uniqueVisits = new HashMap<>();

    // ...
}
```
If the data is not meant to be stored persistently, you could declare a local variable within your servlet methods instead.


```java
@Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        resp.setStatus(200);
        resp.setHeader("Content-Type", "application/json");

        String name = req.getParameter("name");

        HashMap<String, Integer> inputData = new HashMap<>();

        // Use inputData to collect values from the request.
    }
```
