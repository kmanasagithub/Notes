# Module 2: Spring Boot

---

# 1. What is Spring Boot?

## Interview Answer

Spring Boot is an extension of the Spring Framework that simplifies the development of Java applications by providing:

- Auto Configuration
- Embedded Servers
- Starter Dependencies
- Production-ready features

It eliminates most of the manual configuration required in traditional Spring applications, allowing developers to build standalone applications quickly.

Spring Boot applications can be started using a simple `main()` method and do not require an external web server because they come with embedded servers like **Tomcat**.

---

# 2. Why was Spring Boot Introduced?

## Interview Answer

Spring Boot was introduced to solve the problems of traditional Spring applications.

Before Spring Boot, developers had to:

- Write a lot of XML configuration.
- Configure DispatcherServlet manually.
- Configure Tomcat separately.
- Add many dependencies manually.
- Spend significant time on project setup.

Spring Boot automates these tasks using conventions and auto-configuration, allowing developers to focus on business logic.

---

# 3. What is `@SpringBootApplication`?

## Interview Answer

`@SpringBootApplication` is the main annotation used in Spring Boot applications.

It is usually placed on the main class of the application.

It combines three annotations:

```java
@Configuration
@EnableAutoConfiguration
@ComponentScan
```

Together, these annotations:

- Configure the application.
- Enable auto-configuration.
- Scan for Spring Beans.

---

# 4. Explain the Three Annotations Inside `@SpringBootApplication`

## 1. `@Configuration`

`@Configuration` indicates that the class contains Spring configuration.

It is similar to the XML configuration file used in traditional Spring.

It allows defining beans inside the class.

Example:

```java
@Configuration
public class AppConfig {

    @Bean
    public Employee employee() {
        return new Employee();
    }
}
```

---

## 2. `@EnableAutoConfiguration`

`@EnableAutoConfiguration` tells Spring Boot to automatically configure beans based on the dependencies present in the project.

Spring Boot checks the classpath and automatically creates required configurations.

---

## 3. `@ComponentScan`

`@ComponentScan` tells Spring to scan the package and its sub-packages for Spring-managed components.

It scans annotations such as:

- `@Component`
- `@Service`
- `@Repository`
- `@Controller`
- `@RestController`

---

# 5. What is Auto Configuration?

## Interview Answer

Auto Configuration is a feature of Spring Boot that automatically configures Spring Beans based on:

- Dependencies on the classpath
- Existing beans
- Configuration properties

This reduces the need for manual configuration.

---

# 6. What is Embedded Tomcat?

## Interview Answer

Embedded Tomcat is a web server packaged inside the Spring Boot application.

It allows the application to run independently without installing or configuring an external Tomcat server.

---

## Traditional Spring

```
Build WAR

↓

Install Tomcat

↓

Deploy WAR

↓

Application Running
```

---

## Spring Boot

```
Run Main()

↓

Embedded Tomcat Starts

↓

Application Running
```

---

## Other Embedded Servers

Spring Boot supports:

- Tomcat (Default)
- Jetty
- Undertow

---

# 7. How Do You Change the Server Port?

The server port is configured in:

- `application.properties`
- `application.yml`

---

## application.properties

```properties
server.port=9090
```

---

## application.yml

```yaml
server:
  port: 9090
```

---

## Default Port

```
8080
```

---

## Follow-up Question

### Q: What if port 8080 is already in use?

### Answer:

Spring Boot will fail to start with a:

```
Port already in use
```

error.

You can:

- Stop the conflicting process.
- Configure another port.

Example:

```properties
server.port=9090
```

---

# 8. Where Do You Configure Application Properties?

## Interview Answer

Application properties are typically configured in:

```
src/main/resources/application.properties
```

or

```
src/main/resources/application.yml
```

---

These files store application settings such as:

- Server port
- Database connection
- Logging
- Spring Security settings
- Custom application properties

---

Example:

```properties
server.port=8081

spring.datasource.url=jdbc:mysql://localhost:3306/test
spring.datasource.username=root
spring.datasource.password=password
```

---

# 9. Difference Between JAR and WAR in Spring Boot

## Interview Answer

In Java applications, JAR and WAR are packaging formats used to bundle:

- Application code
- Dependencies
- Resources

The main difference is how the application is deployed and executed.

---

# JAR (Java Archive)

## Definition

A JAR is the default packaging format in Spring Boot.

It packages:

- Java classes
- Resources
- Dependencies

into a single executable file.

It comes with an embedded server, allowing it to run directly using:

```bash
java -jar <filename.jar>
```

---

## Advantages of JAR

- Supports standalone deployment.
- Contains embedded server.
- Easy to deploy.
- Preferred for microservices architecture.

---

# WAR (Web Application Archive)

## Definition

A WAR file is used for traditional Java web applications.

It is deployed into an external application server like:

- Tomcat
- WebLogic

---

## WAR Deployment Flow

```
Build WAR

↓

Deploy WAR into External Server

↓

Application Running
```

---

# JAR vs WAR Comparison

| Feature | JAR | WAR |
|---------|-----|-----|
| Full Form | Java Archive | Web Application Archive |
| Server | Embedded Server | External Server |
| Deployment | Standalone | Requires Application Server |
| Spring Boot Preference | Yes | Less preferred |
| Usage | Microservices | Traditional Web Applications |

