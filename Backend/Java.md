# Java
## Streams
`Interface Stream<T>`
A stream is a sequence of elements supporting sequential and parallel aggregate operations.
```Java
// widgets is Collection<widget>
int sum = widgets.stream()
                      .filter(w -> w.getColor() == RED)
                      .mapToInt(w -> w.getWeight())
                      .sum();
```
N.B: This kind of feels similar to LINQ in C#

## Utility Classes
A utility class in Java is a class that provides static methods that are accessible for use across an application. The static methods in utility classes are used for performing common routines in our application.

Utility classes cannot be instantiated and are sometimes stateless without static variables. We declare a utility class as final, and all its methods must be static.

Since we don’t want our utility classes to be instantiated, a private constructor is introduced. Having a private constructor means that Java won’t create a default constructor for our utility class. The constructor can be empty.

```Java
public final class MyUtilityClass {
    private MyUtilityClass() {}

    public static void someHelperMethod() {
        // ...
    }
}
```


# JUnit
## Strict Stubbing
`Strictness` is a new feature of Mockito 2 that drives cleaner tests and better productivity
For example, with strict stubbing, `PotentialStubbingProblem` is thrown if a mocked method is stubbed with some argument in a test, but invoked with a _different_ argument in code.

Example:

```Java
//test method:
 given(mock.getSomething(100)).willReturn(something);

 //code under test:
 Something something = mock.getSomething(50); // <-- stubbing argument mismatch
```

## Failures
 We can assert fail conditions by wrapping the sut in a `try/catch`, and calling `fail()` after where the expected fail is. If the test doesn't actually fail, `fail()` will trigger the test to fail.

# Spring
## News
- `@MockBean` is widely used in Spring Boot unit tests to mock dependencies, but in SB 3.2 this has been deprecated and introduces `@MockitoBean`

## IoC
The main pakcages for Spring's IoC container are `org.springframework.beans` and `org.springframework.context`

Beans are Java objects that are managed by Spring IoC container. The `@Autowired` annotation is used to set up automatic DI, allowing Spring to resolve and inject collaborating beans into a class.

## Controllers and Requests
[Building a REST service](https://spring.io/guides/gs/rest-service)

Controllers in Spring are marked by the `@Controller` annotation.
RESTful controllers are identified by the `@RestController` annotation.

To map requests to controller methods, we use the `@RequestMapping` annotation with the specific shortcuts:
- `@GetMapping`
- `@PostMapping`
- `@PutMapping`
- `@DeleteMapping`
- `@PatchMapping`

## Data interaction
`@Repository` annotation indicates that a class is a repository. Comes with some data persistence and exception handling goodies.

### JOOQ
"jOOQ Object Oriented Querying" lets you build SQL using its DSL

JOOQ generates record objects, which are essentially a list of database columns. Records originating from a concrete db table (or view) are modelled by jOOQ as `TableRecord` or `UpdatableRecord`

## Caching
Spring supports declarative annotation-based caching on methods
- `@Cacheable`: Triggers cache population. Each time the method is called, the cache is checked to see whether the invocation has already been run and does not need to be repeated
    - `cacheNames` (a.k.a `value`): Names of the caches in which results are stored
    - `key`: They key used for the cache key-value store. Most use cases do not need to specify a key as long as parameters have natural keys and implement valid `hashCode()` and `equals()` methods. Otherwise custom keys can be specified using Spring Expression Language: `key="#id"`
    - `unless` and `condition` can be passed in to make caching conditional or veto caching
- `@CacheEvict`: Marks methods that perform cache eviction

### Setup
To enable caching, we need to use the `@EnableCaching` annotation, which will trigger a post-processor that inspects every Spring bean for the presence of caching annotations on public methods. If we use `@EnableCaching` on the main class, Spring will automatically configure a suitable `CacheManager` to serve as a provider, which uses a `ConcurrentHashMap` by default.

Or we can set up our own cache config to implement something like Redis

## Validator
Custom validation: Read https://www.baeldung.com/spring-mvc-custom-validator

## Testing
### MVC Testing
Rather than instantiating a controller, injecting it with dependencies and calling controller methods, Spring has a `MockMvc` class which provides out of the box MVC testing support

By setting up `MockMvc` with `WebApplicationContext`, we can test our actual Spring MVC configuration, and override necessary components with `@MockitoBean`

### Unit Testing
Unit tests that don't need access to the Spring context can use `@Mock` instead of `@MockitoBean` to create a simple mock.

`@InjectMocks` can be used to automatically set up a service with all the depenendencies marked with `@Mock`, removing need to instantiate it in setup.

`verify()` can be used to verify if a mock was called with intended arguments

## Transaction Management
In Spring Boot, the `@Transactional` annotation is used to define the scope of a transaction, either at the class or method level. If any transaction fails, all the changes made to that particular transaction can be rolled back to ensure database consistency

# Lombok
## Builder
https://projectlombok.org/features/Builder

`@Builder` produces a builder for your classes, so that they can be instantiated with code such as:
```Java
@Builder
public class Widget {
    private final String name;
    private final int id;
}
```
```Java
Widget testWidget = Widget.builder()
  .name("foo")
  .id(1)
  .build();
```

Applying `toBuilder = true` generates a method called `toBuilder()` in the class, which when called creates a new builder that starts off with the values of the instance it's called on
```Java
Widget testWidget = Widget.builder()
  .name("foo")
  .id(1)
  .build();

Widget.WidgetBuilder widgetBuilder = testWidget.toBuilder();

Widget newWidget = widgetBuilder.id(2).build();
assertThat(newWidget.getName())
  .isEqualTo("foo");
assertThat(newWidget.getId())
  .isEqualTo(2);
```

## Equals
`@EqualsAndHashCode` generates `hashCode` and `equals` implementations from object fields

By default it will generate an `equals` method that compares all non-static, non-transient fields.

Specific properties can be specified if the comparison only needs to look at those