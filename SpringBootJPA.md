
# @Embedded and @Embeddable in Spring Boot JPA

## What is @Embedded?

`@Embedded` is used to include the fields of one class inside another entity's table without creating a separate database table.

It helps us group related fields together and keep our code clean.


## Why do we use @Embedded?

Suppose an Employee has an Address.

Without `@Embedded`, the Employee class becomes cluttered:

```java
@Entity
public class Employee {

    @Id
    private Long id;

    private String name;

    private String street;
    private String city;
    private String state;
    private String pincode;
}
```

This works, but it is difficult to maintain.

## Using @Embeddable

Create a separate Address class:

```java
import jakarta.persistence.Embeddable;

@Embeddable
public class Address {

    private String street;
    private String city;
    private String state;
    private String pincode;
}
```

The `@Embeddable` annotation tells JPA that this class can be embedded into another entity.

## Using @Embedded

```java
import jakarta.persistence.*;

@Entity
public class Employee {

    @Id
    @GeneratedValue(strategy = GenerationType.AUTO)
    private Long id;

    private String name;

    @Embedded
    private Address address;
}
```

The `@Embedded` annotation tells JPA to include all fields of Address inside the Employee table.


## Database Table Structure

Even though Address is a separate Java class, JPA does NOT create a separate table.

Employee table:

| id | name   | street  | city      | state     | pincode |
| -- | ------ | ------- | --------- | --------- | ------- |
| 1  | Manasa | MG Road | Hyderabad | Telangana | 500001  |

Notice that all Address fields are stored in the Employee table itself.


## Difference Between @Embedded and @OneToOne

### @Embedded

```java
@Embedded
private Address address;
```

Database:

```text
employees
```

Only one table is created.


### @OneToOne

```java
@OneToOne
private Address address;
```

Database:

```text
employees
addresses
```

Two tables are created and linked using a foreign key.

## When Should We Use @Embedded?

Use `@Embedded` when:

* The object has no independent identity.
* The object always belongs to the parent entity.
* You do not need a separate table.

Examples:

* Address
* Audit Information
* Contact Information
* Coordinates
* Bank Details

## Real-World Example

```java
@Embeddable
public class AuditInfo {

    private String createdBy;
    private LocalDateTime createdAt;
    private String updatedBy;
    private LocalDateTime updatedAt;
}
```

```java
@Entity
public class Employee {

    @Id
    private Long id;

    private String name;

    @Embedded
    private AuditInfo auditInfo;
}
```

This is commonly used in enterprise applications.

## Interview Answer

### What is @Embedded?

`@Embedded` is used to include the fields of another class into the current entity's table without creating a separate table.

### What is @Embeddable?

`@Embeddable` marks a class whose fields can be embedded into another entity.

### Does @Embedded create a separate table?

No. The fields are stored in the same table as the parent entity.

### When should we use it?

When we want to group related fields together while keeping them in the same database table.


---

# Enums in Spring Boot JPA - Interview Questions & Answers

## Q1. What is an Enum in Java?

### Answer

An Enum is a special Java type used to represent a fixed set of constants.

Example:

```java
public enum Gender {
    MALE,
    FEMALE,
    OTHER
}
```

Only these values can be used.

## Q2. Why do we use Enums?

### Answer

Enums are used to restrict values to a predefined set.

Without Enum:

```java
private String gender;
```

Possible values:

```text
male
Male
MALE
abc
xyz
```

With Enum:

```java
private Gender gender;
```

Only:

```text
MALE
FEMALE
OTHER
```

are allowed.

Benefits:

* Type Safety
* Better Readability
* Fewer Bugs
* Compile-Time Validation

## Q3. How do we create an Enum?

### Answer

```java
public enum Role {
    USER,
    ADMIN
}
```

## Q4. How do we use an Enum in an Entity?

### Answer

```java
@Entity
public class User {

    @Id
    private Long id;

    @Enumerated(EnumType.STRING)
    private Role role;
}
```

## Q5. What is @Enumerated?

### Answer

`@Enumerated` tells JPA how to store Enum values in the database.

Example:

```java
@Enumerated(EnumType.STRING)
private Role role;
```

## Q6. What are the types of @Enumerated?

### Answer

There are two types:

### EnumType.ORDINAL

```java
@Enumerated(EnumType.ORDINAL)
private Role role;
```

Database:

| Role  | Stored Value |
| ----- | ------------ |
| USER  | 0            |
| ADMIN | 1            |

---

### EnumType.STRING

```java
@Enumerated(EnumType.STRING)
private Role role;
```

Database:

| Role  | Stored Value |
| ----- | ------------ |
| USER  | USER         |
| ADMIN | ADMIN        |


## Q7. Which EnumType should we use?

### Answer

```java
@Enumerated(EnumType.STRING)
```

is recommended.

Reason:

* Easy to read
* Safe when Enum values change
* Commonly used in real projects

## Q8. Why is EnumType.ORDINAL dangerous?

### Answer

Suppose:

```java
public enum Role {
    USER,
    ADMIN
}
```

Database:

```text
USER = 0
ADMIN = 1
```

Later:

```java
public enum Role {
    USER,
    MANAGER,
    ADMIN
}
```

Now:

```text
USER = 0
MANAGER = 1
ADMIN = 2
```

Existing database records become incorrect.


## Q9. What is @ElementCollection?

### Answer

`@ElementCollection` is used when we want to store multiple simple values such as:

* Strings
* Enums
* Numbers

without creating a separate Entity.

Example:

```java
@ElementCollection
private Set<Role> roles;
```

## Q10. Why do we use @ElementCollection with Enum?

### Answer

Because Enum is not an Entity.

Example:

```java
@ElementCollection(fetch = FetchType.EAGER)
@Enumerated(EnumType.STRING)
private Set<Role> roles;
```

This allows a user to have multiple roles.

Example:

```java
USER
ADMIN
```

## Q11. What database tables are created for this code?

### Answer

```java
@ElementCollection(fetch = FetchType.EAGER)
@Enumerated(EnumType.STRING)
private Set<Role> roles;
```

JPA creates:

### app_user

| id | email                                 |
| -- | ------------------------------------- |
| 1  | [sai@gmail.com](mailto:sai@gmail.com) |

### app_user_roles

| user_id | roles |
| ------- | ----- |
| 1       | USER  |
| 1       | ADMIN |

```

