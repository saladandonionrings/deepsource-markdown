# Audit: Hibernate query may be vulnerable to injection attacks
**ID:** `JAVA-A1040` | **Link:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-A1040)

![Critical](https://img.shields.io/badge/severity-critical-red)![Security](https://img.shields.io/badge/type-security-red)

Avoid creating Hibernate SQL queries with strings containing unsanitized input.

Hibernate is a high-level ORM library, but can also handle "raw" SQL queries through the `Session.createQuery()` and `Session.createSQLQuery()` methods. These methods allow one to create [HQL](https://docs.jboss.org/hibernate/orm/3.5/reference/en/html/queryhql.html) and SQL queries respectively.

While both interfaces support parameterization, the possibility of concatenating a query string still exists. This issue will be raised if a hibernate query string appears to be dynamically generated.


## Bad Practice

```java
String userName = request.getParameter("name");
String password = request.getParameter("pass");
// An attacker could freely manipulate the value of userName or password to change the meaning of this query in some way.
List<LoginInfo> infoList = sessionFactory.getCurrentSession().createQuery("from LoginInfo where userName='" + userName + "' and password='" + password + "'").list();
```

## Recommended
Make sure to properly parameterize data in queries to prevent such issues. If you wish to safely specify things like column names or even entity types, consider using the [`Criteria`](https://docs.jboss.org/hibernate/annotations/3.5/api/org/hibernate/Criteria.html) API to do so:


```java
Criteria cr = session.createCriteria(LoginInfo.class);

cr.add(Restrictions.eq("userName", userName));
cr.add(Restrictions.eq("password", password));

List<LoginInfo> infoList = cr.list();
```
Here, care must be taken if column names also need to be varied. Ensure that invalid combinations cannot be used to avoid throwing exceptions unnecessarily.

Otherwise, it may be better to keep the query string constant, while setting only parameters.


```java
String userName = request.getParameter("name");
String password = request.getParameter("pass");

 List<LoginInfo> infoList = sessionFactory.getCurrentSession().createQuery("from LoginInfo where userName = :username and password = :password").setParameter("username", userName).setParameter("password", password).list();
```
