
# @Embedded and @Embeddable in Spring Boot JPA

## What is @Embedded?

`@Embedded` is used to include the fields of one class inside another entity's table without creating a separate database table.

It helps us group related fields together and keep our code clean.

---

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

---

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

---

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

---

## Database Table Structure

Even though Address is a separate Java class, JPA does NOT create a separate table.

Employee table:

| id | name   | street  | city      | state     | pincode |
| -- | ------ | ------- | --------- | --------- | ------- |
| 1  | Manasa | MG Road | Hyderabad | Telangana | 500001  |

Notice that all Address fields are stored in the Employee table itself.

---

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

---

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

---

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

---

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

---

## Interview Answer

### What is @Embedded?

`@Embedded` is used to include the fields of another class into the current entity's table without creating a separate table.

### What is @Embeddable?

`@Embeddable` marks a class whose fields can be embedded into another entity.

### Does @Embedded create a separate table?

No. The fields are stored in the same table as the parent entity.

### When should we use it?

When we want to group related fields together while keeping them in the same database table.
