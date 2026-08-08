# Advanced .NET Interview Preparation Notes

> Continuation of the first 35 interview questions.
>
> These notes cover the additional topics recommended for a **4-year
> .NET developer interview**.
>
> **Interview strategy:** Give the "Interview Answer" first. Use the
> examples and follow-ups when the interviewer asks for more depth.

------------------------------------------------------------------------

# Table of Contents

## 01 - C# Advanced

-   [1. OOP Concepts](#1-oop-concepts)
-   [2. Delegates](#2-delegates)
-   [3. Events](#3-events)
-   [4. Generics](#4-generics)
-   [5. LINQ](#5-linq)
-   [6. Exception Handling](#6-exception-handling)

## 02 - .NET

-   [7. Async and Await](#7-async-and-await)
-   [8. Task vs Thread](#8-task-vs-thread)
-   [9. IEnumerable vs IQueryable](#9-ienumerable-vs-iqueryable)
-   [10. Configuration](#10-configuration)
-   [11. Logging](#11-logging)

## 03 - ASP.NET Core

-   [12. REST API](#12-rest-api)
-   [13. Routing](#13-routing)
-   [14. JWT](#14-jwt)
-   [15. Exception Handling](#15-exception-handling)
-   [16. API Versioning](#16-api-versioning)
-   [17. CORS](#17-cors)

## 04 - Entity Framework Core

-   [18. Change Tracking](#18-change-tracking)
-   [19. Migrations](#19-migrations)
-   [20. Include and ThenInclude](#20-include-and-theninclude)
-   [21. EF Core Performance](#21-ef-core-performance)

## 05 - SQL

-   [22. Joins](#22-joins)
-   [23. Transactions](#23-transactions)
-   [24. Query Optimization](#24-query-optimization)
-   [25. CTE](#25-cte)
-   [26. Window Functions](#26-window-functions)

## 06 - System Design

-   [27. SOLID Principles](#27-solid-principles)
-   [28. Design Patterns](#28-design-patterns)
-   [29. Monolith vs Microservices](#29-monolith-vs-microservices)
-   [30. Caching](#30-caching)
-   [31. Redis](#31-redis)
-   [32. Kafka vs RabbitMQ](#32-kafka-vs-rabbitmq)
-   [33. Scalability](#33-scalability)

## 07 - AWS

-   [34. EC2](#34-ec2)
-   [35. Lambda](#35-lambda)
-   [36. S3](#36-s3)
-   [37. API Gateway](#37-api-gateway)
-   [38. IAM](#38-iam)
-   [39. CloudWatch](#39-cloudwatch)

------------------------------------------------------------------------

# 01 - C# Advanced

## 1. OOP Concepts

### Interview Answer

> Object-Oriented Programming is a programming approach where software
> is designed around objects that contain data and behavior.
>
> The four major OOP concepts are encapsulation, abstraction,
> inheritance and polymorphism.

### 1. Encapsulation

Encapsulation means bundling data and behavior together and controlling
access to internal state.

``` csharp
public class BankAccount
{
    private decimal balance;

    public void Deposit(decimal amount)
    {
        if (amount > 0)
            balance += amount;
    }

    public decimal GetBalance()
    {
        return balance;
    }
}
```

### 2. Abstraction

Hide implementation details and expose only what is required.

``` csharp
public interface IPaymentService
{
    void Pay(decimal amount);
}
```

### 3. Inheritance

A class can reuse or extend behavior from another class.

``` csharp
public class Animal
{
    public void Eat() { }
}

public class Dog : Animal
{
    public void Bark() { }
}
```

### 4. Polymorphism

The same interface or base type can represent different implementations.

``` csharp
public interface INotification
{
    void Send();
}

public class EmailNotification : INotification
{
    public void Send() { }
}

public class SmsNotification : INotification
{
    public void Send() { }
}
```

### Easy Interview Summary

``` text
Encapsulation → Protect data
Abstraction   → Hide implementation
Inheritance   → Reuse behavior
Polymorphism  → One interface, multiple implementations
```

------------------------------------------------------------------------

## 2. Delegates

### Interview Answer

> A delegate is a type-safe reference to a method. It allows methods to
> be passed as parameters and is commonly used for callbacks, events and
> functional programming.

### Example

``` csharp
public delegate void NotificationHandler(string message);

public static void SendNotification(string message)
{
    Console.WriteLine(message);
}

NotificationHandler handler = SendNotification;

handler("Hello");
```

### Built-in Delegates

``` csharp
Action
Func<T>
Predicate<T>
```

Example:

``` csharp
Func<int, int, int> add = (a, b) => a + b;

int result = add(10, 20);
```

### Follow-up

**Why use delegates?**

> Delegates provide a way to pass behavior as a parameter and are useful
> for callbacks, events and flexible APIs.

------------------------------------------------------------------------

## 3. Events

### Interview Answer

> An event is a mechanism built on delegates that allows a class to
> notify other classes when something happens. The class that owns the
> event controls when it is raised, while subscribers can register
> handlers.

### Example

``` csharp
public class OrderService
{
    public event EventHandler? OrderCreated;

    public void CreateOrder()
    {
        // Create order

        OrderCreated?.Invoke(this, EventArgs.Empty);
    }
}
```

Subscriber:

``` csharp
orderService.OrderCreated += OnOrderCreated;

void OnOrderCreated(object? sender, EventArgs e)
{
    Console.WriteLine("Order created");
}
```

### Delegate vs Event

> A delegate can generally be invoked by code that has access to it. An
> event restricts invocation so that only the declaring type can raise
> it.

------------------------------------------------------------------------

## 4. Generics

### Interview Answer

> Generics allow us to define classes, methods or interfaces that work
> with different data types while maintaining compile-time type safety.
>
> They reduce code duplication and avoid unnecessary casting and boxing.

### Example

``` csharp
public class Repository<T>
{
    public void Add(T item)
    {
        // Add item
    }
}
```

Usage:

``` csharp
Repository<Employee> employeeRepository = new();
Repository<Product> productRepository = new();
```

### Generic Method

``` csharp
public T GetValue<T>(T value)
{
    return value;
}
```

### Benefits

-   Type safety
-   Reusability
-   Less casting
-   Better performance in many value-type scenarios

------------------------------------------------------------------------

## 5. LINQ

### Interview Answer

> LINQ stands for Language Integrated Query. It provides a consistent
> way to query collections, databases, XML and other data sources using
> C# syntax.

### Example

``` csharp
var employees = employees
    .Where(e => e.Salary > 50000)
    .OrderBy(e => e.Name)
    .Select(e => e.Name)
    .ToList();
```

### Important Concepts

-   `Where`
-   `Select`
-   `OrderBy`
-   `GroupBy`
-   `Join`
-   `Any`
-   `All`
-   `FirstOrDefault`
-   `SingleOrDefault`

### Deferred Execution

Many LINQ queries execute only when enumerated.

``` csharp
var query = employees.Where(e => e.Salary > 50000);

// Query executes here
var result = query.ToList();
```

### Interview Follow-up

**IEnumerable vs IQueryable?**

> `IEnumerable` is generally used for in-memory enumeration, while
> `IQueryable` can build an expression tree that a provider such as EF
> Core can translate into a database query.

------------------------------------------------------------------------

## 6. Exception Handling

### Interview Answer

> Exception handling allows an application to detect and handle runtime
> errors without crashing unexpectedly. C# provides `try`, `catch`,
> `finally` and `throw` for exception handling.

### Example

``` csharp
try
{
    int result = 10 / number;
}
catch (DivideByZeroException ex)
{
    Console.WriteLine(ex.Message);
}
finally
{
    Console.WriteLine("Cleanup");
}
```

### Best Practices

-   Catch specific exceptions
-   Don't use exceptions for normal control flow
-   Log exceptions appropriately
-   Preserve stack trace when rethrowing

Correct:

``` csharp
throw;
```

Avoid:

``` csharp
throw ex;
```

------------------------------------------------------------------------

# 02 - .NET

## 7. Async and Await

### Interview Answer

> `async` and `await` are used for asynchronous programming in .NET.
> They allow an application to perform I/O-bound operations without
> blocking the calling thread while waiting for the operation to
> complete.
>
> This is especially useful for database calls, HTTP requests and file
> operations.

### Example

``` csharp
public async Task<Employee> GetEmployeeAsync(int id)
{
    return await repository.GetEmployeeAsync(id);
}
```

### Important

`async/await` does not automatically create a new thread.

For I/O-bound work:

``` text
Request
  ↓
Start I/O
  ↓
Thread is available
  ↓
I/O completes
  ↓
Continuation resumes
```

### Follow-up

**When should you use async?**

> Primarily for I/O-bound operations such as database, network and file
> operations.

------------------------------------------------------------------------

## 8. Task vs Thread

### Interview Answer

> A Thread is an actual operating-system execution thread, while a Task
> represents an asynchronous operation or unit of work. Tasks are
> higher-level and are commonly scheduled using the thread pool.
>
> In modern .NET applications, Tasks and async/await are generally
> preferred for asynchronous operations instead of manually creating
> threads.

  Thread                        Task
  ----------------------------- ------------------------------------------
  Lower-level                   Higher-level abstraction
  Represents execution thread   Represents operation/work
  More expensive                More lightweight
  Manual management possible    Works well with async/await
  OS resource                   Usually scheduled by runtime/thread pool

------------------------------------------------------------------------

## 9. IEnumerable vs IQueryable

### Interview Answer

> `IEnumerable<T>` is used to enumerate data in memory, while
> `IQueryable<T>` represents a query that can be translated by a query
> provider, such as EF Core, into a database query.
>
> With `IQueryable`, filtering can be pushed to the database before data
> is materialized.

### Example

``` csharp
IQueryable<Employee> query = db.Employees;

query = query.Where(e => e.Salary > 50000);

var employees = await query.ToListAsync();
```

The filter can be translated into SQL.

### Important

Avoid unnecessarily calling:

``` csharp
.ToList()
```

too early, because it materializes the data and subsequent operations
may happen in memory.

------------------------------------------------------------------------

## 10. Configuration

### Interview Answer

> ASP.NET Core provides a configuration system that can read settings
> from sources such as `appsettings.json`, environment variables,
> command-line arguments and secret stores.
>
> Configuration can be accessed through `IConfiguration` or strongly
> typed options using the Options pattern.

### Example

`appsettings.json`:

``` json
{
  "ConnectionStrings": {
    "DefaultConnection": "..."
  }
}
```

Access:

``` csharp
var connectionString =
    configuration.GetConnectionString("DefaultConnection");
```

### Best Practice

> Environment-specific or sensitive values should not be hard-coded.
> Secrets should be stored in an appropriate secret-management system.

------------------------------------------------------------------------

## 11. Logging

### Interview Answer

> ASP.NET Core provides built-in logging abstractions through
> `ILogger<T>`. Logging helps us monitor application behavior, diagnose
> failures and troubleshoot production issues.

### Example

``` csharp
public class OrderService
{
    private readonly ILogger<OrderService> _logger;

    public OrderService(ILogger<OrderService> logger)
    {
        _logger = logger;
    }

    public void CreateOrder()
    {
        _logger.LogInformation("Creating order");
    }
}
```

### Common Log Levels

``` text
Trace
Debug
Information
Warning
Error
Critical
```

### Best Practice

> Avoid logging passwords, tokens, sensitive personal information or
> other secrets.

------------------------------------------------------------------------

# 03 - ASP.NET Core

## 12. REST API

### Interview Answer

> REST is an architectural style for designing web APIs around
> resources. REST APIs commonly use HTTP methods such as GET, POST, PUT,
> PATCH and DELETE and typically exchange data using JSON.

### HTTP Methods

  Method   Purpose
  -------- -------------------------
  GET      Retrieve resource
  POST     Create resource
  PUT      Replace/update resource
  PATCH    Partial update
  DELETE   Delete resource

### Example

``` http
GET /api/employees/10
```

Response:

``` json
{
  "id": 10,
  "name": "John"
}
```

### REST Principles

-   Resource-oriented
-   Stateless communication
-   Standard HTTP methods
-   Standard HTTP status codes
-   Representation such as JSON

------------------------------------------------------------------------

## 13. Routing

### Interview Answer

> Routing determines which endpoint or controller action should handle
> an incoming HTTP request based on the request URL and HTTP method.

### Attribute Routing

``` csharp
[ApiController]
[Route("api/[controller]")]
public class EmployeesController : ControllerBase
{
    [HttpGet("{id}")]
    public IActionResult Get(int id)
    {
        return Ok();
    }
}
```

Request:

``` http
GET /api/employees/10
```

### Common Routing Approaches

-   Conventional routing
-   Attribute routing

------------------------------------------------------------------------

## 14. JWT

### Interview Answer

> JWT stands for JSON Web Token. It is a compact token format commonly
> used for authentication and authorization between clients and APIs.
>
> A JWT contains claims and is digitally signed so that the receiving
> system can verify its integrity.

### Structure

``` text
Header.Payload.Signature
```

Example concept:

``` text
xxxxx.yyyyy.zzzzz
```

### Important

A JWT is normally:

-   Signed
-   Not encrypted by default
-   Used to carry claims
-   Sent using the Authorization header

``` http
Authorization: Bearer <token>
```

### Important Interview Point

> We should never put sensitive secrets or passwords in JWT claims just
> because the token is signed. The payload is typically readable by
> whoever has the token.

------------------------------------------------------------------------

## 15. Exception Handling in ASP.NET Core

### Interview Answer

> In ASP.NET Core, exceptions can be handled centrally using
> exception-handling middleware. Centralized handling avoids duplicating
> try/catch blocks across controllers and allows us to return consistent
> error responses.

### Example

``` csharp
app.UseExceptionHandler("/error");
```

A custom exception-handling middleware can also be implemented.

### Recommended Response

Use a consistent error format, such as `ProblemDetails`.

### Benefits

-   Centralized handling
-   Consistent API responses
-   Better logging
-   Cleaner controllers

------------------------------------------------------------------------

## 16. API Versioning

### Interview Answer

> API versioning allows us to evolve an API without immediately breaking
> existing clients. Different versions can support different contracts
> while existing consumers continue using the version they depend on.

### Common Approaches

``` text
URL:
 /api/v1/employees
 /api/v2/employees

Query string:
 /api/employees?version=2

Header:
 X-API-Version: 2
```

### Interview Point

> The choice depends on organizational standards and client
> requirements. URL versioning is simple and easy to understand.

------------------------------------------------------------------------

## 17. CORS

### Interview Answer

> CORS stands for Cross-Origin Resource Sharing. It is a browser
> security mechanism that controls whether a web application from one
> origin can make requests to an API hosted on another origin.

### Example

``` csharp
builder.Services.AddCors(options =>
{
    options.AddPolicy("Frontend", policy =>
    {
        policy
            .WithOrigins("https://example.com")
            .AllowAnyHeader()
            .AllowAnyMethod();
    });
});
```

Then:

``` csharp
app.UseCors("Frontend");
```

### Important

> CORS is primarily a browser-side security mechanism. It does not
> replace API authentication or authorization.

------------------------------------------------------------------------

# 04 - Entity Framework Core

## 18. Change Tracking

### Interview Answer

> Change tracking is an EF Core feature where the DbContext keeps track
> of entity instances and their state. When `SaveChanges()` is called,
> EF Core determines what INSERT, UPDATE or DELETE operations are
> required.

### Entity States

``` text
Added
Modified
Deleted
Unchanged
Detached
```

### Example

``` csharp
var employee = await context.Employees.FindAsync(10);

employee.Name = "John";

await context.SaveChangesAsync();
```

EF Core detects the changed property and generates the appropriate
UPDATE.

### AsNoTracking

For read-only queries:

``` csharp
var employees = await context.Employees
    .AsNoTracking()
    .ToListAsync();
```

This can reduce tracking overhead.

------------------------------------------------------------------------

## 19. Migrations

### Interview Answer

> EF Core migrations provide a way to incrementally evolve the database
> schema based on changes to the application's entity model.

### Common Commands

``` bash
dotnet ef migrations add InitialCreate
```

``` bash
dotnet ef database update
```

### Typical Flow

``` text
Change Entity
     ↓
Create Migration
     ↓
Review Migration
     ↓
Apply Migration
     ↓
Database Schema Updated
```

### Important

> Migrations should be reviewed before production deployment, especially
> when they contain destructive schema changes.

------------------------------------------------------------------------

## 20. Include and ThenInclude

### Interview Answer

> `Include` is used in EF Core to eagerly load related entities.
> `ThenInclude` is used to load a related entity further down the
> relationship chain.

### Example

``` csharp
var orders = await context.Orders
    .Include(o => o.Customer)
    .ThenInclude(c => c.Address)
    .ToListAsync();
```

This loads:

``` text
Order
  ↓
Customer
  ↓
Address
```

### Important

> Eager loading should be used carefully because loading large object
> graphs can generate expensive queries and unnecessary data transfer.

------------------------------------------------------------------------

## 21. EF Core Performance

### Interview Answer

> EF Core performance can be improved by selecting only required
> columns, using `AsNoTracking` for read-only queries, avoiding
> unnecessary `Include`, filtering at the database level and ensuring
> appropriate database indexes exist.

### Example

Avoid:

``` csharp
var employees = await context.Employees
    .ToListAsync();
```

if only two columns are needed.

Prefer:

``` csharp
var employees = await context.Employees
    .Select(e => new
    {
        e.Id,
        e.Name
    })
    .ToListAsync();
```

### Other Techniques

-   Use pagination
-   Avoid N+1 queries
-   Use appropriate indexes
-   Avoid premature `ToList()`
-   Use async database APIs
-   Use `AsNoTracking()` for read-only queries
-   Inspect generated SQL for complex queries

------------------------------------------------------------------------

# 05 - SQL

## 22. Joins

### Interview Answer

> SQL joins are used to combine rows from multiple tables based on a
> related column or condition.

### INNER JOIN

Returns matching rows from both tables.

``` sql
SELECT e.Name, d.Name
FROM Employees e
INNER JOIN Departments d
    ON e.DepartmentId = d.Id;
```

### LEFT JOIN

Returns all rows from the left table and matching rows from the right
table.

``` sql
SELECT e.Name, d.Name
FROM Employees e
LEFT JOIN Departments d
    ON e.DepartmentId = d.Id;
```

### RIGHT JOIN

Returns all rows from the right table and matching rows from the left
table.

### FULL OUTER JOIN

Returns matching rows plus unmatched rows from both tables.

### Easy Memory

``` text
INNER → Only matches
LEFT  → Everything from left
RIGHT → Everything from right
FULL  → Everything from both
```

------------------------------------------------------------------------

## 23. Transactions

### Interview Answer

> A transaction is a group of database operations treated as a single
> unit of work. Either all operations succeed, or the transaction can be
> rolled back so that the database does not remain in a partially
> updated state.

### ACID

``` text
A → Atomicity
C → Consistency
I → Isolation
D → Durability
```

### Example

``` sql
BEGIN TRANSACTION;

UPDATE Accounts
SET Balance = Balance - 100
WHERE Id = 1;

UPDATE Accounts
SET Balance = Balance + 100
WHERE Id = 2;

COMMIT;
```

If something fails:

``` sql
ROLLBACK;
```

### Interview Example

> A bank transfer is a classic example. Debit and credit should succeed
> together.

------------------------------------------------------------------------

## 24. Query Optimization

### Interview Answer

> Query optimization is the process of improving a SQL query so that it
> executes efficiently while returning the same result.

### Common Techniques

-   Create appropriate indexes
-   Avoid `SELECT *`
-   Filter data early
-   Avoid unnecessary joins
-   Check execution plans
-   Use appropriate predicates
-   Avoid functions on indexed columns when they prevent efficient index
    usage
-   Return only required rows using pagination

### Example

Instead of:

``` sql
SELECT *
FROM Employees;
```

Use:

``` sql
SELECT Id, Name, Department
FROM Employees
WHERE Department = 'IT';
```

### Interview Point

> I would first inspect the execution plan and identify expensive scans,
> joins, sorts or lookups before changing the query or indexes.

------------------------------------------------------------------------

## 25. CTE

### Interview Answer

> CTE stands for Common Table Expression. It is a temporary named result
> set that exists for the duration of a single SQL statement. It
> improves readability and is particularly useful for complex queries
> and recursive queries.

### Example

``` sql
WITH EmployeeCTE AS
(
    SELECT Id, Name, Department
    FROM Employees
    WHERE Department = 'IT'
)
SELECT *
FROM EmployeeCTE;
```

### Recursive CTE

Useful for hierarchical data:

``` text
Manager
  ↓
Employee
  ↓
Subordinate
```

------------------------------------------------------------------------

## 26. Window Functions

### Interview Answer

> Window functions perform calculations across a set of related rows
> without collapsing those rows into a single result like a GROUP BY
> does.

### Example

``` sql
SELECT
    Name,
    Department,
    Salary,
    ROW_NUMBER() OVER (
        PARTITION BY Department
        ORDER BY Salary DESC
    ) AS RowNumber
FROM Employees;
```

### Common Window Functions

-   `ROW_NUMBER()`
-   `RANK()`
-   `DENSE_RANK()`
-   `LAG()`
-   `LEAD()`
-   `SUM() OVER()`
-   `AVG() OVER()`

### Common Interview Question

**Find the second-highest salary in each department.**

Window functions are a clean approach:

``` sql
WITH RankedEmployees AS
(
    SELECT
        Name,
        Department,
        Salary,
        DENSE_RANK() OVER (
            PARTITION BY Department
            ORDER BY Salary DESC
        ) AS SalaryRank
    FROM Employees
)
SELECT *
FROM RankedEmployees
WHERE SalaryRank = 2;
```

------------------------------------------------------------------------

# 06 - System Design

## 27. SOLID Principles

### Interview Answer

> SOLID is a set of five object-oriented design principles that help
> create software that is maintainable, flexible and easier to test.

### S --- Single Responsibility Principle

> A class should have one reason to change.

Bad:

``` text
OrderService
 ├── Create order
 ├── Send email
 ├── Generate PDF
 └── Save audit log
```

Better:

``` text
OrderService
EmailService
PdfService
AuditService
```

### O --- Open/Closed Principle

> Software entities should be open for extension but closed for
> modification.

### L --- Liskov Substitution Principle

> A derived class should be substitutable for its base class without
> breaking expected behavior.

### I --- Interface Segregation Principle

> Clients should not be forced to depend on methods they do not use.

### D --- Dependency Inversion Principle

> High-level modules should not depend directly on low-level concrete
> implementations. Both should depend on abstractions.

Example:

``` csharp
public class OrderService
{
    private readonly IPaymentService _paymentService;

    public OrderService(IPaymentService paymentService)
    {
        _paymentService = paymentService;
    }
}
```

### Easy Memory

``` text
S → Single Responsibility
O → Open/Closed
L → Liskov Substitution
I → Interface Segregation
D → Dependency Inversion
```

------------------------------------------------------------------------

## 28. Design Patterns

### Interview Answer

> Design patterns are reusable solutions to commonly occurring software
> design problems. They provide proven approaches for structuring code
> but should be used when they solve an actual design problem rather
> than simply for the sake of using patterns.

### Common Patterns in .NET

#### Factory

Creates objects without exposing the creation logic.

``` csharp
public interface IPayment
{
}

public class PaymentFactory
{
    public static IPayment Create(string type)
    {
        // Create appropriate implementation
        throw new NotImplementedException();
    }
}
```

#### Strategy

Allows interchangeable algorithms.

``` text
Payment
 ├── CreditCardStrategy
 ├── PayPalStrategy
 └── UPIStrategy
```

#### Repository

Abstracts data access.

#### Singleton

Ensures a single instance.

> In ASP.NET Core, prefer the built-in DI container's singleton lifetime
> rather than manually implementing a singleton in most cases.

------------------------------------------------------------------------

## 29. Monolith vs Microservices

### Interview Answer

> A monolithic application is deployed as a single application unit,
> while a microservices architecture splits the system into
> independently deployable services organized around business
> capabilities.
>
> Microservices can provide independent scaling and deployment, but they
> also introduce distributed-system complexity such as network failures,
> observability, service communication and data consistency.

### Monolith

``` text
┌─────────────────────┐
│   Single Application│
│                     │
│ Orders              │
│ Payments            │
│ Users               │
│ Notifications       │
└─────────────────────┘
```

### Microservices

``` text
Order Service
     ↓
Payment Service

User Service
     ↓
Notification Service
```

### When Microservices Make Sense

-   Independent scaling requirements
-   Independent deployment
-   Large teams with clear ownership
-   Strong business boundaries
-   Different technology/scaling needs

### Important

> Microservices are not automatically better. A modular monolith can be
> a better choice when the system and team are small or the domain
> boundaries are not mature.

------------------------------------------------------------------------

## 30. Caching

### Interview Answer

> Caching stores frequently accessed data temporarily so that future
> requests can be served faster without repeatedly accessing the
> original data source.

### Example

Without cache:

``` text
API → Database → Response
```

With cache:

``` text
API → Cache → Response

If cache miss:
API → Cache → Database → Cache → Response
```

### Types

-   In-memory cache
-   Distributed cache
-   HTTP/browser cache
-   Database caching

### Benefits

-   Lower latency
-   Reduced database load
-   Better scalability

### Challenges

-   Cache invalidation
-   Stale data
-   Memory usage
-   Consistency

------------------------------------------------------------------------

## 31. Redis

### Interview Answer

> Redis is an in-memory data store commonly used for caching,
> distributed locks, counters, session storage and other low-latency use
> cases.

### Example

``` text
Application
    ↓
Redis
    ↓
Cache Hit → Return data

Cache Miss
    ↓
Database
    ↓
Redis
    ↓
Return data
```

### Common Use Cases

-   Distributed caching
-   Session storage
-   Rate limiting
-   Counters
-   Pub/Sub
-   Distributed locks

### Interview Point

> Redis should not automatically be treated as a replacement for the
> primary database. It is commonly used alongside a durable database.

------------------------------------------------------------------------

## 32. Kafka vs RabbitMQ

### Interview Answer

> Kafka is a distributed event streaming platform designed for
> high-throughput event streams and durable event storage. RabbitMQ is a
> message broker commonly used for reliable asynchronous messaging and
> routing.
>
> The choice depends on the use case, throughput, ordering, retention
> and messaging requirements.

  Kafka                      RabbitMQ
  -------------------------- ------------------------------------
  Event streaming platform   Message broker
  High throughput            Flexible routing
  Durable event log          Queue-based messaging
  Consumers track offsets    Messages are consumed/acknowledged
  Good for event streams     Good for work queues and messaging

### Kafka Example

``` text
Producer
   ↓
Kafka Topic
   ↓
Consumer Group
   ↓
Consumers
```

### RabbitMQ Example

``` text
Producer
   ↓
Exchange
   ↓
Queue
   ↓
Consumer
```

------------------------------------------------------------------------

## 33. Scalability

### Interview Answer

> Scalability is the ability of a system to handle increasing load by
> adding resources or improving its architecture.

### Vertical Scaling

Increase resources of one machine.

``` text
2 CPU → 8 CPU
4 GB RAM → 32 GB RAM
```

### Horizontal Scaling

Add more instances.

``` text
          Load Balancer
          /     |     \
       API 1  API 2  API 3
```

### Common Scalability Techniques

-   Horizontal scaling
-   Load balancing
-   Caching
-   Database indexing
-   Database read replicas
-   Asynchronous processing
-   Message queues
-   CDN
-   Stateless services

### Interview Point

> Stateless APIs are easier to scale horizontally because any healthy
> instance can handle a request.

------------------------------------------------------------------------

# 07 - AWS

## 34. EC2

### Interview Answer

> Amazon EC2, or Elastic Compute Cloud, provides resizable virtual
> servers in AWS. We can choose the instance type, operating system,
> storage, networking and security configuration based on our workload.

### Common Components

-   AMI
-   Instance type
-   EBS
-   Security Group
-   Key pair
-   VPC
-   IAM role
-   Elastic IP

### Security Group

> A security group acts as a virtual firewall controlling inbound and
> outbound traffic for an EC2 instance.

### Example Architecture

``` text
Internet
   ↓
Load Balancer
   ↓
EC2 Instances
   ↓
Database
```

### Interview Follow-up

**How do you deploy a .NET API on EC2?**

Typical approach:

``` text
.NET Application
      ↓
Publish
      ↓
EC2 Instance
      ↓
Install .NET Runtime / Hosting
      ↓
Configure Application
      ↓
Reverse Proxy / Load Balancer
      ↓
Internet
```

------------------------------------------------------------------------

## 35. Lambda

### Interview Answer

> AWS Lambda is a serverless compute service that runs code in response
> to events without requiring us to manage servers directly.
>
> AWS automatically handles the underlying infrastructure, and we
> generally pay based on execution and resource usage.

### Example Triggers

-   API Gateway
-   S3 events
-   SQS
-   EventBridge
-   Scheduled events

### Flow

``` text
API Gateway
     ↓
Lambda
     ↓
Business Logic
     ↓
Database
```

### Advantages

-   Serverless
-   Automatic scaling
-   Event-driven
-   No server management
-   Pay-per-use model

### Limitations

-   Execution limits
-   Cold starts
-   Stateless execution model
-   Runtime/resource constraints

### EC2 vs Lambda

  -----------------------------------------------------------------------
  EC2                                 Lambda
  ----------------------------------- -----------------------------------
  Virtual server                      Serverless function

  You manage OS/server                AWS manages infrastructure

  Long-running workloads              Event-driven workloads

  Pay for provisioned resources       Pay based on execution/resource
                                      usage

  More control                        Less infrastructure control
  -----------------------------------------------------------------------

------------------------------------------------------------------------

## 36. S3

### Interview Answer

> Amazon S3, or Simple Storage Service, is an object storage service
> used to store and retrieve files and other objects at scale.

### Common Use Cases

-   Images
-   Videos
-   Documents
-   Backups
-   Logs
-   Static website assets
-   Data lake storage

### Structure

``` text
S3
 └── Bucket
      ├── object1
      ├── object2
      └── folder/object3
```

### Important Concepts

-   Bucket
-   Object
-   Object key
-   Storage classes
-   Versioning
-   Lifecycle policies
-   Encryption
-   Access policies

### Interview Point

> S3 is object storage, not a traditional file system or relational
> database.

------------------------------------------------------------------------

## 37. API Gateway

### Interview Answer

> Amazon API Gateway is a managed service that provides an entry point
> for APIs. It can route requests to services such as Lambda and can
> provide features such as authentication integration, throttling,
> monitoring and request handling.

### Architecture

``` text
Client
   ↓
API Gateway
   ↓
Lambda / EC2 / Other Service
```

### Common Features

-   API routing
-   Authentication/authorization integration
-   Throttling
-   Monitoring
-   Request/response handling
-   API lifecycle management

### API Gateway + Lambda

A common serverless architecture is:

``` text
Client
  ↓
API Gateway
  ↓
Lambda
  ↓
DynamoDB / RDS / Other Service
```

------------------------------------------------------------------------

## 38. IAM

### Interview Answer

> AWS IAM, or Identity and Access Management, controls who can access
> AWS resources and what actions they are allowed to perform.

### Main Concepts

``` text
User
Role
Policy
Group
```

### Policy Example

A policy defines permissions such as:

``` text
Allow:
  s3:GetObject

Resource:
  specific S3 bucket/object
```

### Best Practice

> Follow the principle of least privilege: grant only the permissions
> required to perform the task.

### IAM Role

> A role is commonly assumed by AWS services such as EC2 or Lambda so
> that applications can access other AWS resources without storing
> long-lived access keys in application code.

------------------------------------------------------------------------

## 39. CloudWatch

### Interview Answer

> Amazon CloudWatch is an AWS monitoring and observability service used
> to collect metrics, logs and events from AWS resources and
> applications.

### Common Uses

-   Application logs
-   EC2 metrics
-   Lambda logs
-   Alarms
-   Dashboards
-   Monitoring resource health

### Example

``` text
Application
    ↓
CloudWatch Logs
    ↓
Logs / Metrics
    ↓
Alarm
    ↓
Notification
```

### Interview Example

> If a Lambda function starts failing, I can inspect its CloudWatch logs
> and metrics to identify errors, latency or resource-related problems.

------------------------------------------------------------------------

# Quick Revision

  ------------------------------------------------------------------------
  \#                      Topic                    One-Line Interview
                                                   Answer
  ----------------------- ------------------------ -----------------------
  1                       OOP                      Encapsulation,
                                                   abstraction,
                                                   inheritance and
                                                   polymorphism

  2                       Delegates                Type-safe references to
                                                   methods

  3                       Events                   Delegate-based
                                                   notification mechanism

  4                       Generics                 Reusable, type-safe
                                                   code

  5                       LINQ                     Query data using C#
                                                   syntax

  6                       Exception Handling       Handle runtime errors
                                                   safely

  7                       Async/Await              Non-blocking
                                                   asynchronous
                                                   programming

  8                       Task vs Thread           High-level operation vs
                                                   execution thread

  9                       IEnumerable/IQueryable   In-memory enumeration
                                                   vs provider-based query

  10                      Configuration            Application settings
                                                   from multiple sources

  11                      Logging                  Application diagnostics
                                                   and observability

  12                      REST API                 Resource-oriented HTTP
                                                   API

  13                      Routing                  Maps HTTP requests to
                                                   endpoints

  14                      JWT                      Signed token containing
                                                   claims

  15                      Exception Handling       Centralized API error
                                                   handling

  16                      API Versioning           Evolve API contracts
                                                   without breaking
                                                   clients

  17                      CORS                     Browser cross-origin
                                                   access control

  18                      Change Tracking          EF Core tracks entity
                                                   state changes

  19                      Migrations               Version database schema
                                                   changes

  20                      Include/ThenInclude      Eagerly load related
                                                   entities

  21                      EF Performance           Optimize queries,
                                                   tracking and data
                                                   transfer

  22                      Joins                    Combine rows from
                                                   related tables

  23                      Transactions             Group operations into
                                                   one atomic unit

  24                      Query Optimization       Reduce query cost while
                                                   preserving results

  25                      CTE                      Named temporary result
                                                   set for one statement

  26                      Window Functions         Calculate across rows
                                                   without grouping them
                                                   away

  27                      SOLID                    Five principles for
                                                   maintainable OOP design

  28                      Design Patterns          Reusable solutions to
                                                   common design problems

  29                      Microservices            Independently
                                                   deployable
                                                   business-oriented
                                                   services

  30                      Caching                  Store frequently used
                                                   data for faster access

  31                      Redis                    In-memory store
                                                   commonly used for
                                                   low-latency data

  32                      Kafka/RabbitMQ           Event streaming vs
                                                   message brokering

  33                      Scalability              Ability to handle
                                                   increasing load

  34                      EC2                      Resizable virtual
                                                   servers

  35                      Lambda                   Serverless event-driven
                                                   compute

  36                      S3                       Scalable object storage

  37                      API Gateway              Managed API entry point

  38                      IAM                      AWS identity and
                                                   permissions

  39                      CloudWatch               AWS monitoring, logs,
                                                   metrics and alarms
  ------------------------------------------------------------------------

------------------------------------------------------------------------

# Most Important Follow-Up Questions

Before an interview, make sure you can explain these without notes:

### C

-   Why use interface vs abstract class?
-   Method overloading vs overriding?
-   `ref` vs `out` vs `in`?
-   `const` vs `readonly`?
-   `string` vs `StringBuilder`?
-   `==` vs `.Equals()`?
-   `IEnumerable` vs `IQueryable`?
-   `Task` vs `Thread`?
-   What happens internally when using `await`?

### ASP.NET Core

-   Explain the request pipeline.
-   Explain DI lifetimes.
-   Middleware vs filters?
-   Authentication vs authorization?
-   How does JWT validation work?
-   How do you implement global exception handling?
-   How do you secure an API?
-   How do you handle API versioning?
-   How do you improve API performance?

### EF Core

-   Tracking vs `AsNoTracking()`?
-   Lazy vs eager vs explicit loading?
-   What is N+1 query?
-   How does EF Core translate LINQ to SQL?
-   What are migrations?
-   How do you optimize EF Core queries?

### SQL

-   INNER JOIN vs LEFT JOIN?
-   Clustered vs non-clustered index?
-   WHERE vs HAVING?
-   DELETE vs TRUNCATE vs DROP?
-   CTE vs temporary table?
-   RANK vs DENSE_RANK vs ROW_NUMBER?
-   What is a deadlock?
-   How do you troubleshoot a slow query?
-   Explain ACID.

### System Design

-   Monolith vs microservices?
-   How would you design a high-traffic API?
-   How would you handle traffic spikes?
-   Where would you use Redis?
-   Kafka vs RabbitMQ?
-   How would you design a scalable ticket-booking system?
-   How would you prevent duplicate bookings?
-   How would you handle distributed transactions?

### AWS

-   EC2 vs Lambda?
-   Lambda cold start?
-   What is an IAM role?
-   Security Group vs IAM?
-   S3 vs EBS?
-   How do you deploy a .NET API on AWS?
-   How does API Gateway work with Lambda?
-   How do you monitor an application using CloudWatch?
-   How would you make an AWS application highly available?

------------------------------------------------------------------------

# Interview Preparation Priority

For a 4-year .NET backend/full-stack interview, prioritize these topics
first:

``` text
HIGH PRIORITY
────────────────────────────
1. OOP + SOLID
2. Dependency Injection
3. Async/Await
4. LINQ
5. IEnumerable vs IQueryable
6. Middleware
7. Authentication + JWT
8. Exception Handling
9. Entity Framework Core
10. SQL Joins + Indexes
11. Transactions
12. REST APIs
13. Microservices
14. Redis / Caching
15. Kafka / RabbitMQ
16. AWS EC2
17. AWS Lambda

MEDIUM PRIORITY
────────────────────────────
18. Delegates / Events
19. Generics
20. Filters
21. API Versioning
22. CORS
23. EF Migrations
24. CTE / Window Functions
25. Design Patterns
26. S3
27. API Gateway
28. IAM
29. CloudWatch
```

# Interview Answer Formula

For technical questions, use this structure:

``` text
1. Definition
2. How it works
3. Simple code/example
4. Real-world use case
5. Advantages / limitations
6. Follow-up if asked
```

### Example

If asked **"What is Redis?"**

Don't answer only:

> Redis is an in-memory database.

Instead:

> Redis is an in-memory data store commonly used for caching and other
> low-latency use cases. In a .NET API, I can first check Redis for
> frequently accessed data. If there is a cache miss, I retrieve the
> data from the database and populate Redis. This reduces database load
> and improves response time. Redis can also be used for distributed
> locks, counters and rate limiting.

That style is much stronger in an interview because it shows **concept +
implementation + real-world experience**.
