# Persistent objects should not be returned from methods
**ID:** `JAVA-S1066` | **Link:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-S1066)

![Critical](https://img.shields.io/badge/severity-critical-red)![Security](https://img.shields.io/badge/type-security-red)

Returning persistent objects from methods should be avoided as much as possible.

APIs that allow returning entity objects risk accidentally leaking the application's business logic to the outside world. Even worse, such APIs may enable attackers to tamper with persistent objects by using a loophole in the application's security. For these reasons, it is best to avoid returning entity objects from methods.


## Bad Practice

```java
@Entity
public class Book {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    private String name;
}

// Bad! `Book` is an `@Entity`.
public Book getBook(Long id) {
    return bookRepository.findById(id);
}
```

## Recommended
Use Data Transfer Objects (DTOs) to pass around data between methods/components.


```java
public class BookDTO {
    private Long id;
    private String name;
}

public BookDTO getBook(Long id) {
    Book book = bookRepository.findById(id);
    // Use a utility method to map `@Entity` to a DTO.
    return converToBookDto(book);
}
```
