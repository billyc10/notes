# Java (Spring)
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

