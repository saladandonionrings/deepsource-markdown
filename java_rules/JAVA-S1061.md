# Request handler method accepts persistent object as argument
**ID:** `JAVA-S1061` | **Link:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-S1061)

![Critical](https://img.shields.io/badge/severity-critical-red)![Security](https://img.shields.io/badge/type-security-red)

Spring request handlers should not allow persistent objects (`@Entity` and `@Document`) to be passed through arguments.

Spring automatically binds request parameters to arguments of request handling methods annotated with `@RequestMapping`, `@GetMapping`, `@PostMapping` etc. Persistent objects, i.e. instances of classes annotated with `@Entity` or `@Document`, are modified by a persistence framework such as Hibernate.

Having persistent objects as arguments to request handling methods is dangerous because it might allow malicious users to craft input that could beat Spring's security mechanisms. If this practice is followed, in certain cases it might be possible to modify the fields of a table in an unexpected manner.


## Bad Practice

```java
@Entity
public class Book {}

@Controller
public class SomeController {
    @PostMapping
    public String saveBook(Book book) {
        bookRepository.save(book);
    }
}
```

## Recommended
Consider introducing a [Data Transfer Object (DTO)](https://stackoverflow.com/a/35079306).


```java
public class BookDTO {}

@Controller
public class SomeController {
    @PostMapping
    public String saveBook(BookDTO bookDTO) {
        Book book = new Book();
        // ... map fields manually between `bookDTO` and `book`.
        bookRepository.save(book);
    }
}
```
