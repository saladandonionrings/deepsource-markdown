# Servlets should not use mutable fields without synchronization
**ID:** `JAVA-E0128` | **Lien:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-E0128)

![Critical](https://img.shields.io/badge/severity-critical-red) ![Bug Risk](https://img.shields.io/badge/type-bug_risk-green)

A web server generally only creates one instance of servlet or JSP class (i.e., treats the class as a Singleton), and will have multiple threads invoke methods on that instance to service multiple simultaneous requests.


## Bad Practice

```java
class MyServlet extends HttpServlet {

    private HashMap<String, User> users; // This field may be left open to concurrent modification.

    // ...

    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        resp.setStatus(200);
        resp.setHeader("Content-Type", "application/json");

        String name = req.getParameter("name");

        users.put(name, ...); // This access is not synchronized and could result in concurrent modification of users.
    }
}
```
Accessing such variables without synchronizing on them could allow `ConcurrentModificationException` s. This could also result in race conditions occurring between threads that modify the concerned field.


## Recommended
Consider using some form of synchronization to ensure that such variables can be accessed safely in a concurrent context.


```java
private synchronized doOperationOnUsers(String name) {
        // users is only modified within this method.

        users.put(name, ...);
    }
```
