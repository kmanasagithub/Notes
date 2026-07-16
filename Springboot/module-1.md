# Module 1: Spring Framework

# 1. What is Spring Framework?

## Interview Answer

Spring Framework is an open-source Java framework used to build enterprise applications. It simplifies Java development by providing features like:

- Dependency Injection (DI)
- Inversion of Control (IoC)
- Aspect-Oriented Programming (AOP)
- Transaction Management
- Spring Security
- Database Integration
- Spring MVC
- Easy Testing

The main goal of Spring is to reduce boilerplate code and create **loosely coupled, maintainable, and testable applications**.

---

## Simple Explanation

Imagine you're building a house.

### Without Spring

- You buy the bricks.
- You buy the cement.
- You hire workers.
- You manage everything yourself.

### With Spring

A construction company manages all these tasks.

You only tell them what you need.

Spring works like that construction company. It creates objects, manages dependencies, and handles configuration so you can focus on writing business logic.

---

## Why Spring?

Before Spring:

- Large XML configuration files
- Tight coupling
- Difficult testing
- Complex object creation

Spring solved these problems by providing dependency management and simplified configuration.

---

## Features

- Dependency Injection (DI)
- IoC Container
- Aspect-Oriented Programming (AOP)
- Transaction Management
- Spring MVC
- Spring Security
- Spring Data JPA
- Microservices Support

---

## Example

### Without Spring

```java
StudentService service = new StudentService();
```

You create the object manually.

### With Spring

```java
@Autowired
private StudentService service;
```

Spring automatically creates and injects the object.

---

## Follow-up Question

### Q: Is Spring Framework and Spring Boot the same?

**Answer:**

No.

- **Spring Framework** is the core framework.
- **Spring Boot** is built on top of Spring Framework to reduce configuration and speed up development.

---

# 2. What are the Features of Spring?

## Interview Answer

The main features of Spring are:

- Dependency Injection (DI)
- Inversion of Control (IoC)
- Aspect-Oriented Programming (AOP)
- Spring MVC
- Transaction Management
- Spring Security
- Data Access
- Hibernate/JPA Integration
- Microservices Support
- Easy Testing

---

# 3. What is Dependency Injection (DI)?

## Interview Answer

Dependency Injection (DI) is a design pattern used in Spring to achieve **loose coupling** between classes.

Instead of creating dependent objects inside a class, the Spring container automatically injects the required dependencies.

---

## Types of Dependency Injection

### 1. Constructor Injection

The dependency is provided through the constructor.

```java
@Component
class Car {

    private Engine engine;

    @Autowired
    public Car(Engine engine) {
        this.engine = engine;
    }
}
```

### Advantages

- Recommended by Spring
- Mandatory dependencies
- Easier unit testing
- Immutable objects

---

### 2. Setter Injection

The dependency is injected using a setter method after object creation.

```java
@Component
class Car {

    private Engine engine;

    @Autowired
    public void setEngine(Engine engine) {
        this.engine = engine;
    }
}
```

### Advantages

- Optional dependencies
- Dependency can be changed later

---

### 3. Field Injection

The dependency is injected directly into the field.

```java
@Component
class Car {

    @Autowired
    private Engine engine;
}
```

### Note

Although commonly used in examples, **Constructor Injection is recommended** over Field Injection because it improves testability and immutability.

---

# 4. What is Inversion of Control (IoC)?

## Interview Answer

Inversion of Control (IoC) is a principle where the control of object creation and dependency management is transferred from the programmer to the Spring Framework.

Instead of creating objects manually using the `new` keyword, Spring creates and manages them.

---

## Example

Without IoC:

```java
StudentService service = new StudentService();
```

With IoC:

```java
@Autowired
private StudentService service;
```

Spring creates the object automatically.

---

# 5. What is IoC Container?

## Interview Answer

The **Spring IoC Container** creates, configures, manages, and destroys Spring Beans.

Instead of creating objects manually, developers let the container manage them.

---

## Types of IoC Containers

### 1. BeanFactory

- Basic IoC container
- Lightweight
- Lazy Initialization (creates bean only when needed)

```java
BeanFactory factory =
    new XmlBeanFactory(new ClassPathResource("beans.xml"));
```

---

### 2. ApplicationContext

Most commonly used IoC container.

Additional features:

- Event propagation
- Internationalization (i18n)
- Annotation support
- Bean lifecycle management

```java
ApplicationContext context =
    new ClassPathXmlApplicationContext("beans.xml");
```

---

# 6. What is a Bean in Spring?

A **Spring Bean** is an object that is created, configured, and managed by the Spring IoC container.

Spring also manages the complete lifecycle of the bean.

---

## Beans can be created using

- `@Component`
- `@Service`
- `@Repository`
- `@Controller`
- `@Bean`
- XML Configuration (`<bean>`)

---

# 7. What is Bean Scope?

Bean Scope defines:

- How many instances of a bean should be created.
- How long those instances should live.

---

## Spring Bean Scopes

### 1. Singleton (Default)

Only **one instance** of the bean is created for the entire Spring container.

- Default scope
- Shared across the application
- Saves memory

```java
@Component
@Scope("singleton")
class Car { }
```

---

### 2. Prototype

A new object is created every time the bean is requested.

```java
@Component
@Scope("prototype")
class Car { }
```

---

### 3. Request Scope

One bean is created for each HTTP request.

- Available only in web applications
- Destroyed after the response

```java
@Component
@Scope("request")
class UserRequest { }
```

---

### 4. Session Scope

One bean instance is created for each HTTP session.

All requests in the same session use the same object.

```java
@Component
@Scope("session")
class UserSession { }
```

---

### 5. Application Scope

One bean instance is created for the entire web application.

Shared across all users and requests.

```java
@Component
@Scope("application")
class AppData { }
```

---

# 8. What is @Autowired?

## Interview Answer

`@Autowired` is a Spring annotation used for **automatic dependency injection**.

Spring searches for the required bean in the IoC container and injects it automatically.

This eliminates the need to manually create objects using the `new` keyword.

---

## Example

```java
@RestController
public class StudentController {

    @Autowired
    private StudentService service;

}
```

# 9. What are `@Component`, `@Service`, `@Repository`, and `@Controller`?

## Interview Answer

`@Component`, `@Service`, `@Repository`, and `@Controller` are **stereotype annotations** in Spring.

They tell the Spring IoC container to detect the class during **component scanning** and register it as a **Spring Bean**.

The main difference between them is **their purpose in the application architecture**.

---

# 1. `@Component`

## Definition

`@Component` is the **generic stereotype annotation**.

It is used for any Spring-managed class that does not specifically belong to the service, repository, or controller layer.

### Purpose

- Registers a class as a Spring Bean.
- Used for helper classes.
- Used for utility classes.
- Used for validators.
- Used for converters.

### Example

```java
@Component
public class EmailValidator {

    public boolean isValid(String email) {
        return email.contains("@");
    }
}
```

### Interview Point

- Generic stereotype annotation.
- Parent annotation of `@Service`, `@Repository`, and `@Controller`.

---

# 2. `@Service`

## Definition

`@Service` is a specialization of `@Component` used for the **Business Logic Layer**.

### Purpose

- Contains business logic.
- Calls repository methods.
- Performs calculations.
- Performs validations.
- Processes application logic.

### Example

```java
@Service
public class EmployeeService {

    public String getEmployee() {
        return "Employee Details";
    }
}
```

### Why not use `@Component`?

Technically, you can.

However, using `@Service` clearly indicates that the class belongs to the **Service Layer**, making the code easier to understand and maintain.

---

# 3. `@Repository`

## Definition

`@Repository` is a specialization of `@Component` used for the **Data Access Layer (DAO)**.

### Purpose

- Interacts with the database.
- Performs CRUD operations.
- Works with JPA.
- Works with Hibernate.
- Works with JDBC.

### Example

```java
@Repository
public class EmployeeRepository {

    public Employee findById(int id) {
        return null;
    }
}
```

### Special Feature

Unlike `@Component`, `@Repository` enables **Exception Translation**.

Spring automatically converts database-specific exceptions into its own consistent `DataAccessException` hierarchy.

---

# 4. `@Controller`

## Definition

`@Controller` is a specialization of `@Component` used in the **Presentation Layer**.

### Purpose

- Handles incoming HTTP requests.
- Returns a View (JSP, Thymeleaf, etc.).

### Example

```java
@Controller
public class HomeController {

    @GetMapping("/")
    public String home() {
        return "index";
    }
}
```

---

# What is `@RestController`?

In Spring Boot REST APIs, `@RestController` is more commonly used.

### Example

```java
@RestController
public class EmployeeController {

    @GetMapping("/employee")
    public String getEmployee() {
        return "John";
    }
}
```

---

## `@RestController` = `@Controller` + `@ResponseBody`

```java
@Controller
@ResponseBody
```

It returns the **response body directly** (such as JSON or plain text) instead of returning a view.

---

