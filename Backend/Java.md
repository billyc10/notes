# Java (Spring)
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

