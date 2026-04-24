# Audit: MongoDB queries using operators like `$where` may be a security risk
**ID:** `JAVA-A1054` | **Lien:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-A1054)

![Critical](https://img.shields.io/badge/severity-critical-red) ![Security](https://img.shields.io/badge/type-security-red)

A MongoDB query appears to contain a dynamically evaluated operator (such as `$where` ) and also accepts untrusted input.

Consider using a declarative query and allow only values being compared to be set directly from input, not operators themselves.

MongoDB treats queries in the same way it treats data, as document objects. These document objects are hierarchical JSON-like data structures (In fact, they are serialized as [BSON](https://www.mongodb.com/basics/bson) objects), having string keys and object values. Due to the declarative style, combined with the type safety of the query format in Java, MongoDB queries cannot be hijacked through string injection like SQL queries can. However, this advantage can be negated when using specific operator strings as the key of a query parameter.

MongoDB allows one to perform query operations using operator keys, which are represented with a `$` prefix. Examples of operators are `$exists` , `$gt` and `$mod` .

Clients for NoSQL databases such as MongoDB can be just as susceptible to injection attacks as SQL database clients are, due to dynamically evaluated operators such as `$where` and `$expr` .


## Bad Practice

```java
BasicDBObject query = new BasicDBObject();

query.put("$expr", BsonDocument.parse(req.getParameter("query"));
```
Here, [ `$expr` ](https://www.mongodb.com/docs/manual/reference/operator/query/expr/#mongodb-query-op.-expr) is a query operator that allows one to compose multiple operations together to create complex queries. If its value is directly retrieved from externally controlled data such as a request parameter, it may be possible to change the meaning of the query to retrieve or modify data.


## Recommended
In security, allow-lists are more preferable to deny-lists, due to how specific they can be. If possible, narrow down to the absolute minimum the behaviors that are desired within a query, and use external input only to select the behavior required for the specific purpose. Avoid using operators such as [ `$where` ](https://www.mongodb.com/docs/manual/reference/operator/query/where/#mongodb-query-op.-where) to evaluate query data from external sources, and instead only use external values as primitive data.


```java
int queryType = Integer.parseInt(req.getParameter("query_id"));

if (queryType <= 0 || queryType > LAST_QUERY_ID)
    throw new IllegalArgumentException(queryType);

switch (queryType) {
    case USER_INFO_QUERY -> {
        String name = req.getParameter("user");
        BasicDBObject query = new BasicDBObject();

        // This query can only check if there is a user name matching the supplied user name.
        query.put("user_name", name);
    }
    // ...
}
```
Another useful tool is to use a data mapping library such as [Morphia](https://morphia.dev/landing/index.html) to convert MongoDB documents directly into Java objects, and [Critter](https://morphia.dev/critter/4.1/index.html) to programmatically create queries in MongoDB.

A third option is to use the newer [collection API](https://www.mongodb.com/docs/drivers/java/sync/current/quick-reference/#quick-reference) to find relevant documents in the database. This may be a good approach in newer MongoDB versions, as the number of dependencies required will reduce.

