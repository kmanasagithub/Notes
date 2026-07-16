# Spring Boot Annotations – Interview Notes

## 1. `@Controller`

Marks the class as a **controller** in the Spring MVC framework.

- Handles incoming HTTP requests.
- Processes the request.
- Returns a **View** (JSP, Thymeleaf, HTML) or a response.

---

## 2. `@RestController`

`@RestController` is a combination of:

- `@Controller`
- `@ResponseBody`

Every method returns **data (JSON/XML)** directly in the HTTP response instead of returning a view.

Used for building **REST APIs**.

Example:

```java
@RestController
public class UserController {

    @GetMapping("/hello")
    public String hello() {
        return "Hello";
    }
}
```

---

## 3. `@RequestMapping`

Used to map a controller method (or an entire controller) to a specific URL pattern.

It can handle multiple HTTP methods such as:

- GET
- POST
- PUT
- DELETE
- PATCH

Example:

```java
@RequestMapping("/users")
public class UserController {
}
```

---

## 4. `@RequestMapping` Shortcut Annotations

| Annotation | HTTP Method | Usage |
|------------|------------|------|
| `@GetMapping` | GET | Fetch resource(s) |
| `@PostMapping` | POST | Create a new resource |
| `@PutMapping` | PUT | Update entire resource |
| `@PatchMapping` | PATCH | Partial update of a resource |
| `@DeleteMapping` | DELETE | Delete a resource |
| `@RequestMapping` | Any / Multiple | Specify using `method=` parameter |

---

# 5. `@PathVariable`

`@PathVariable` is used to extract values from the **URL path** and pass them into a controller method.

It is useful when URLs contain dynamic values.

Example:

```java
@GetMapping("/user/{id}")
public String getUser(@PathVariable int id) {
    return "User ID is " + id;
}
```

Request:

```
GET /user/101
```

Output:

```
User ID is 101
```

---

# 6. `@RequestParam`

`@RequestParam` is used to read **query parameters** from a URL.

It extracts values that appear after the `?`.

Example URL:

```
http://localhost:8080/hello?name=Manasa
```

### Basic Example

```java
@GetMapping("/hello")
public String sayHello(@RequestParam String name) {
    return "Hello " + name;
}
```

### Optional Parameter

```java
@GetMapping("/hello")
public String sayHello(@RequestParam(required = false) String name) {
    return "Hello " + name;
}
```

If `name` is missing, the value becomes `null`.

### Default Value

```java
@GetMapping("/hello")
public String sayHello(
    @RequestParam(defaultValue = "Guest") String name) {

    return "Hello " + name;
}
```

If no value is supplied, the output becomes:

```
Hello Guest
```

---

# 7. `@ResponseBody`

`@ResponseBody` tells Spring to send the returned value directly as the HTTP response instead of returning a view.

Spring automatically converts Java objects into JSON.

Example:

```java
@GetMapping("/user")
@ResponseBody
public String getUser() {
    return "Hello User";
}
```

Output:

```
Hello User
```

### Shortcut

Instead of writing `@ResponseBody` on every method, use:

```java
@RestController
public class MyController {

}
```

`@RestController` automatically applies `@ResponseBody` to every method.

---

# 8. `@RequestHeader`

`@RequestHeader` is used to read values from HTTP request headers.

Headers usually contain metadata like:

- Authorization Token
- Content-Type
- Accept
- User-Agent

Example Request:

```
GET /hello

Authorization: Bearer abc123
```

Example:

```java
@GetMapping("/hello")
public String hello(
    @RequestHeader("Authorization") String token) {

    return "Token is " + token;
}
```

Output:

```
Token is Bearer abc123
```

---

# 9. `@RequestBody`

`@RequestBody` reads data from the HTTP request body and converts it into a Java object.

It is mainly used with:

- POST
- PUT
- PATCH

Client sends JSON:

```json
{
    "name": "Manasa",
    "age": 22
}
```

Controller:

```java
@PostMapping("/user")
public String addUser(@RequestBody User user) {
    return user.getName();
}
```

How it works:

```
Client JSON
        ↓
Spring Boot
        ↓
Java Object
        ↓
Method receives object
```

---

# 10. Difference between `@Controller` and `@RestController`

| `@Controller` | `@RestController` |
|---------------|-------------------|
| Used for Spring MVC applications | Used for REST APIs |
| Returns View (HTML/JSP/Thymeleaf) | Returns JSON/XML |
| Needs `@ResponseBody` to return JSON | `@ResponseBody` is automatically applied |

---

# 11. Difference between `@RequestBody` and `@ResponseBody`

| `@RequestBody` | `@ResponseBody` |
|---------------|----------------|
| Reads data sent by the client | Sends data back to the client |
| Converts JSON → Java Object | Converts Java Object → JSON |

---

# 12. `@Qualifier`

`@Qualifier` is used when multiple beans of the same type exist.

It tells Spring **exactly which bean should be injected**.

Example:

```java
@RestController
public class UserController {

    private final NotificationService service;

    public UserController(
            @Qualifier("emailService")
            NotificationService service) {

        this.service = service;
    }
}
```

If there are two implementations:

- `EmailService`
- `SmsService`

Using:

```java
@Qualifier("emailService")
```

Spring injects only `EmailService`.

---

# 13. `@Primary`

`@Primary` marks one bean as the **default bean** when multiple beans of the same type exist.

Example:

```java
@Service
@Primary
public class EmailService implements NotificationService {

}
```

Now:

```java
@Autowired
private NotificationService service;
```

Spring automatically injects:

```
EmailService
```

because it is marked as the primary bean.

### Interview Point

- `@Primary` → Default bean
- `@Qualifier` → Specific bean

`@Qualifier` has higher priority than `@Primary`.

---

# 14. `@Value`

`@Value` is used to read values from configuration.

It can read values from:

- `application.properties`
- `application.yml`
- Environment Variables
- JVM System Properties

Example (`application.properties`):

```properties
app.name=Student Management System
app.version=1.0
```

Java:

```java
@Component
public class AppInfo {

    @Value("${app.name}")
    private String appName;

    @Value("${app.version}")
    private String version;
}
```

After injection:

```
appName = Student Management System

version = 1.0
```

---

# Quick Interview Summary

| Annotation | Purpose |
|------------|---------|
| `@Controller` | Returns View (MVC) |
| `@RestController` | Returns JSON/XML (REST API) |
| `@RequestMapping` | Maps URLs to controller methods |
| `@GetMapping` | GET request |
| `@PostMapping` | POST request |
| `@PutMapping` | PUT request |
| `@PatchMapping` | PATCH request |
| `@DeleteMapping` | DELETE request |
| `@PathVariable` | Reads values from URL path |
| `@RequestParam` | Reads query parameters |
| `@RequestHeader` | Reads HTTP headers |
| `@RequestBody` | Converts JSON → Java Object |
| `@ResponseBody` | Converts Java Object → JSON |
| `@Qualifier` | Inject a specific bean |
| `@Primary` | Marks the default bean |
| `@Value` | Reads configuration values |
