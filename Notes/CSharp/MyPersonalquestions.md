# C# / .NET / ASP.NET / SQL / Angular Interview Notes

> Interview preparation notes for a .NET developer.
>
> **Answering strategy:** Start with the **Interview Answer**, give the
> example if asked, and then handle the follow-up questions.

------------------------------------------------------------------------

## Table of Contents

1.  [What is C#?](#1-what-is-c)
2.  [What is CLR?](#2-what-is-clr)
3.  [What is Garbage Collection?](#3-what-is-garbage-collection)
4.  [What is a Stored Procedure?](#4-what-is-a-stored-procedure)
5.  [What is Indexing?](#5-what-is-indexing)
6.  [What is a View?](#6-what-is-a-view)
7.  [What is a Trigger?](#7-what-is-a-trigger)
8.  [What is AJAX?](#8-what-is-ajax)
9.  [What does \$(document).ready() do?](#9-what-does-documentready-do)
10. [What are Aggregate Functions?](#10-what-are-aggregate-functions)
11. [What is an Output Parameter?](#11-what-is-an-output-parameter)
12. [What are Pure and Impure
    Pipes?](#12-what-are-pure-and-impure-pipes)
13. [What are Components and Modules in
    Angular?](#13-what-are-components-and-modules-in-angular)
14. [What is Middleware?](#14-what-is-middleware)
15. [What is Dependency Injection?](#15-what-is-dependency-injection)
16. [How do you implement Authentication in ASP.NET Web
    API?](#16-how-do-you-implement-authentication-in-aspnet-web-api)
17. [What are Filters in ASP.NET
    Core?](#17-what-are-filters-in-aspnet-core)
18. [What is jQuery?](#18-what-is-jquery)
19. [What is Two-Way Data Binding?](#19-what-is-two-way-data-binding)
20. [What are Decorators in
    Angular?](#20-what-are-decorators-in-angular)
21. [What are Directives in
    Angular?](#21-what-are-directives-in-angular)
22. [What is DbContext?](#22-what-is-dbcontext)
23. [What is Entity Framework?](#23-what-is-entity-framework)
24. [ViewBag vs ViewData vs
    TempData](#24-viewbag-vs-viewdata-vs-tempdata)
25. [Stored Procedure vs Function](#25-stored-procedure-vs-function)
26. [Managed vs Unmanaged Code](#26-managed-vs-unmanaged-code)
27. [Boxing and Unboxing](#27-boxing-and-unboxing)
28. [Types of Classes in C#](#28-types-of-classes-in-c)
29. [Struct vs Class](#29-struct-vs-class)
30. [What is an Enum?](#30-what-is-an-enum)
31. [DataSet vs DataTable](#31-dataset-vs-datatable)
32. [What are Extension Methods?](#32-what-are-extension-methods)
33. [What are Constraints in SQL
    Server?](#33-what-are-constraints-in-sql-server)
34. [What is a Recursive Stored
    Procedure?](#34-what-is-a-recursive-stored-procedure)
35. [Authentication vs
    Authorization](#35-authentication-vs-authorization)

------------------------------------------------------------------------

# C# / .NET

## 1. What is C#?

### Interview Answer

> C# is a modern, strongly typed and object-oriented programming
> language developed by Microsoft. It is primarily used with the .NET
> platform to build web applications, REST APIs, desktop applications,
> cloud applications and enterprise software.
>
> C# supports OOP concepts such as encapsulation, inheritance,
> polymorphism and abstraction. It also provides features such as
> generics, LINQ, exception handling, delegates, async/await and
> automatic memory management through garbage collection.

### Key Points

-   Object-oriented
-   Strongly typed
-   Type-safe
-   Garbage collection
-   Exception handling
-   Generics
-   LINQ
-   Async/await
-   Delegates and events
-   Cross-platform with modern .NET

### Example

``` csharp
public class Employee
{
    public string Name { get; set; }

    public void Display()
    {
        Console.WriteLine(Name);
    }
}
```

### Common Follow-up

**Is C# compiled or interpreted?**

C# is compiled. The C# compiler converts source code into Intermediate
Language (IL). At runtime, the JIT compiler converts IL into native
machine code.

------------------------------------------------------------------------

## 2. What is CLR?

**CLR = Common Language Runtime**

### Interview Answer

> CLR is the execution environment provided by .NET for running managed
> code. It provides services such as garbage collection, memory
> management, exception handling, type safety, thread management and JIT
> compilation.
>
> When C# code is compiled, it is converted into Intermediate Language.
> At runtime, the CLR's JIT compiler converts the IL into native machine
> code and executes it.

### Execution Flow

``` text
C# Source Code
      ↓
C# Compiler
      ↓
Intermediate Language (IL)
      ↓
CLR
      ↓
JIT Compiler
      ↓
Native Machine Code
      ↓
CPU
```

### Responsibilities

-   JIT compilation
-   Garbage collection
-   Memory management
-   Exception handling
-   Type safety
-   Thread management
-   Assembly loading

### Follow-up: What is JIT?

> JIT stands for Just-In-Time compiler. It converts Intermediate
> Language into native machine code at runtime.

------------------------------------------------------------------------

## 3. What is Garbage Collection?

### Interview Answer

> Garbage Collection is the automatic memory management mechanism
> provided by the .NET runtime. It identifies objects that are no longer
> reachable by the application and reclaims the memory occupied by those
> objects.
>
> This means developers generally do not need to manually release
> managed memory.

### Generations

``` text
Generation 0 → Short-lived objects
Generation 1 → Medium-lived objects
Generation 2 → Long-lived objects
```

### Example

``` csharp
public void CreateEmployee()
{
    Employee employee = new Employee();
}
```

If no references to `employee` remain after the method finishes, the
object can eventually become eligible for garbage collection.

### Important Interview Point

> Garbage Collection manages managed memory. Unmanaged resources such as
> files, streams and database connections should still be disposed
> properly using `IDisposable` and `using`.

``` csharp
using var stream = File.OpenRead("data.txt");
```

------------------------------------------------------------------------

# SQL Server

## 4. What is a Stored Procedure?

### Interview Answer

> A stored procedure is a reusable set of SQL statements stored in SQL
> Server. It can accept input parameters, perform multiple database
> operations and return result sets or output parameters.
>
> Stored procedures are useful for encapsulating database logic, reusing
> operations and controlling database access.

### Example

``` sql
CREATE PROCEDURE GetEmployee
    @EmployeeId INT
AS
BEGIN
    SELECT *
    FROM Employees
    WHERE Id = @EmployeeId;
END;
```

Execute:

``` sql
EXEC GetEmployee @EmployeeId = 10;
```

### Advantages

-   Reusable
-   Encapsulates database logic
-   Supports parameters
-   Can contain transactions
-   Can return result sets
-   Can provide controlled database access

------------------------------------------------------------------------

## 5. What is Indexing?

### Interview Answer

> An index is a database structure used to improve data retrieval
> performance. It allows SQL Server to locate rows more efficiently
> instead of scanning the entire table.
>
> However, indexes consume storage and add overhead to INSERT, UPDATE
> and DELETE operations because the indexes must also be maintained.

### Example

``` sql
CREATE INDEX IX_Employee_Email
ON Employees(Email);
```

### Common Types

#### Clustered Index

Determines the logical/physical organization of the table's data rows.

A table can have only one clustered index.

#### Non-Clustered Index

Maintains a separate index structure containing indexed values and
references to the underlying rows.

A table can have multiple non-clustered indexes.

### Follow-up

**Why not create indexes on every column?**

> Because indexes consume storage and increase write overhead. Indexes
> should be created based on actual query patterns and performance
> requirements.

------------------------------------------------------------------------

## 6. What is a View?

### Interview Answer

> A view is a database object that represents the result of a SELECT
> query. It provides an abstraction over underlying tables and can be
> used to simplify complex queries or expose only selected data.

### Example

``` sql
CREATE VIEW EmployeeView
AS
SELECT Id, Name, Department
FROM Employees;
```

Usage:

``` sql
SELECT *
FROM EmployeeView;
```

### Advantages

-   Simplifies complex queries
-   Provides abstraction
-   Can restrict exposed columns/rows
-   Reusable
-   Can improve security in appropriate scenarios

### Important

> A normal view generally stores the query definition rather than a
> separate copy of the result data.

------------------------------------------------------------------------

## 7. What is a Trigger?

### Interview Answer

> A trigger is a special database object that automatically executes
> when a specified database event occurs, such as INSERT, UPDATE or
> DELETE.
>
> Triggers are commonly used for auditing, maintaining derived data or
> enforcing certain database-level rules.

### Example

``` sql
CREATE TRIGGER trg_Employee_Insert
ON Employees
AFTER INSERT
AS
BEGIN
    INSERT INTO EmployeeAudit(EmployeeId, Name)
    SELECT Id, Name
    FROM inserted;
END;
```

### Types

-   AFTER trigger
-   INSTEAD OF trigger

### Important

> A trigger executes automatically. We do not normally call it
> explicitly like a stored procedure.

------------------------------------------------------------------------

## 10. What are Aggregate Functions?

### Interview Answer

> Aggregate functions perform calculations on multiple rows and return a
> single result.

### Common Functions

``` text
COUNT()
SUM()
AVG()
MIN()
MAX()
```

### Example

``` sql
SELECT
    COUNT(*) AS EmployeeCount,
    AVG(Salary) AS AverageSalary,
    MAX(Salary) AS MaximumSalary
FROM Employees;
```

### With GROUP BY

``` sql
SELECT Department, COUNT(*) AS EmployeeCount
FROM Employees
GROUP BY Department;
```

------------------------------------------------------------------------

## 11. What is an Output Parameter?

### Interview Answer

> An output parameter allows a stored procedure to return a value
> through a parameter to the calling code. The procedure assigns a value
> to the output parameter, and the caller can read it after execution.

### Example

``` sql
CREATE PROCEDURE GetEmployeeCount
    @Count INT OUTPUT
AS
BEGIN
    SELECT @Count = COUNT(*)
    FROM Employees;
END;
```

Usage:

``` sql
DECLARE @Total INT;

EXEC GetEmployeeCount @Total OUTPUT;

SELECT @Total;
```

------------------------------------------------------------------------

# Angular / JavaScript / jQuery

## 8. What is AJAX?

### Interview Answer

> AJAX stands for Asynchronous JavaScript and XML. It is a technique for
> communicating with a server asynchronously without reloading the
> entire webpage.
>
> Although the name contains XML, modern applications commonly use JSON
> for data exchange.

### Example

``` javascript
fetch('/api/employees')
    .then(response => response.json())
    .then(data => console.log(data));
```

### Benefits

-   No full page reload
-   Better user experience
-   Asynchronous server communication
-   Commonly uses JSON

------------------------------------------------------------------------

## 9. What does `$(document).ready()` do?

### Interview Answer

> `$(document).ready()` is a jQuery method that executes a callback
> after the HTML DOM has been loaded and is ready to be manipulated.

### Example

``` javascript
$(document).ready(function () {
    console.log("DOM is ready");
});
```

### Modern JavaScript Equivalent

``` javascript
document.addEventListener('DOMContentLoaded', function () {
    console.log('DOM is ready');
});
```

------------------------------------------------------------------------

## 12. What are Pure and Impure Pipes?

### Interview Answer

> A pure pipe executes when Angular detects a change in its input value
> or input reference. This makes pure pipes more efficient.
>
> An impure pipe can execute during every change detection cycle, so it
> can have a higher performance cost.

### Pure Pipe

``` typescript
@Pipe({
    name: 'myPipe',
    pure: true
})
```

### Impure Pipe

``` typescript
@Pipe({
    name: 'myPipe',
    pure: false
})
```

### Important

> Pipes are pure by default.

------------------------------------------------------------------------

## 13. What are Components and Modules in Angular?

### Component

> A component controls a part of the application's UI. It generally
> contains a TypeScript class, HTML template and styles.

``` typescript
@Component({
    selector: 'app-user',
    templateUrl: './user.component.html'
})
export class UserComponent {
}
```

### Module

> An Angular module is a mechanism for organizing related Angular
> functionality such as components, directives and pipes.

### Modern Angular Note

> Modern Angular also supports standalone components, which can reduce
> the need for traditional NgModules.

------------------------------------------------------------------------

## 18. What is jQuery?

### Interview Answer

> jQuery is a JavaScript library that simplifies common client-side
> tasks such as DOM manipulation, event handling, AJAX calls and
> animations.
>
> It was widely used before modern JavaScript APIs and frontend
> frameworks became common.

### Example

``` javascript
$("#btnSubmit").click(function () {
    alert("Clicked");
});
```

### Interview Point

> In modern applications, frameworks such as Angular, React and Vue are
> commonly used for complex frontend applications, while native
> JavaScript APIs can handle many tasks that previously required jQuery.

------------------------------------------------------------------------

## 19. What is Two-Way Data Binding?

### Interview Answer

> Two-way data binding means changes in the UI are reflected in the
> component's data, and changes in the component's data are reflected
> back in the UI.
>
> In Angular, it is commonly implemented using `[(ngModel)]`.

### Example

``` html
<input [(ngModel)]="name">

<p>{{ name }}</p>
```

### Concept

``` text
Component  ←→  UI
```

------------------------------------------------------------------------

## 20. What are Decorators in Angular?

### Interview Answer

> Decorators are TypeScript features used by Angular to attach metadata
> to classes, properties, methods or parameters. Angular uses this
> metadata to understand how those elements should behave.

### Example

``` typescript
@Component({
    selector: 'app-user',
    templateUrl: './user.component.html'
})
export class UserComponent {
}
```

Here, `@Component` is a decorator.

### Common Decorators

``` text
@Component
@Injectable
@Input
@Output
@Directive
@Pipe
```

------------------------------------------------------------------------

## 21. What are Directives in Angular?

### Interview Answer

> Directives are classes that allow us to add behavior to DOM elements
> or change their structure.
>
> Angular has structural directives that change the DOM structure and
> attribute directives that modify the behavior or appearance of an
> existing element.

### Example

``` html
<div *ngIf="isLoggedIn">
    Welcome
</div>
```

``` html
<div [ngClass]="className">
    Hello
</div>
```

### Types

-   Component
-   Structural directive
-   Attribute directive

### Modern Angular Note

> Modern Angular also provides built-in control flow such as `@if` and
> `@for`.

------------------------------------------------------------------------

# ASP.NET / Entity Framework

## 14. What is Middleware?

### Interview Answer

> Middleware is a component in the ASP.NET Core HTTP request pipeline
> that can inspect, modify or handle HTTP requests and responses.
>
> A middleware can perform some logic and then pass the request to the
> next middleware, or it can terminate the pipeline.

### Example

``` csharp
app.UseAuthentication();
app.UseAuthorization();

app.MapControllers();
```

### Pipeline

``` text
Request
   ↓
Middleware 1
   ↓
Middleware 2
   ↓
Middleware 3
   ↓
Controller
   ↓
Response
```

### Common Middleware

-   Exception handling
-   Authentication
-   Authorization
-   Logging
-   CORS
-   HTTPS redirection

------------------------------------------------------------------------

## 15. What is Dependency Injection?

### Interview Answer

> Dependency Injection is a design pattern where an object's
> dependencies are provided from outside instead of the object creating
> those dependencies itself.
>
> It reduces tight coupling, improves testability and makes applications
> easier to maintain.

### Without DI

``` csharp
public class OrderService
{
    private EmailService _email = new EmailService();
}
```

### With DI

``` csharp
public class OrderService
{
    private readonly IEmailService _email;

    public OrderService(IEmailService email)
    {
        _email = email;
    }
}
```

Registration:

``` csharp
builder.Services.AddScoped<IEmailService, EmailService>();
```

### DI Lifetimes

  Lifetime    Meaning
  ----------- -------------------------------------------
  Transient   New instance each time requested
  Scoped      One instance per request/scope
  Singleton   One instance for the application lifetime

------------------------------------------------------------------------

## 16. How do you implement Authentication in ASP.NET Web API?

### Interview Answer

> A common approach is JWT-based authentication. The client
> authenticates with an identity provider or authentication server and
> receives a JWT. The client sends that token with subsequent API
> requests in the Authorization header. ASP.NET Core authentication
> middleware validates the token before the request reaches the
> protected endpoint.

### Flow

``` text
Client
   ↓
Login
   ↓
Identity Provider
   ↓
JWT Token
   ↓
Client
   ↓
Authorization: Bearer <token>
   ↓
Authentication Middleware
   ↓
Controller
```

### Example

``` csharp
builder.Services
    .AddAuthentication("Bearer")
    .AddJwtBearer(options =>
    {
        options.Authority = "https://identity-server";
        options.Audience = "my-api";
    });
```

Protected endpoint:

``` csharp
[Authorize]
[HttpGet]
public IActionResult GetUsers()
{
    return Ok();
}
```

------------------------------------------------------------------------

## 17. What are Filters in ASP.NET Core?

### Interview Answer

> Filters allow us to execute custom logic before or after specific
> stages of MVC or Web API request processing. They are useful for
> cross-cutting concerns such as authorization, logging, validation,
> exception handling and action-related processing.

### Common Filter Types

-   Authorization Filter
-   Resource Filter
-   Action Filter
-   Exception Filter
-   Result Filter

### Example

``` csharp
public class LoggingFilter : IActionFilter
{
    public void OnActionExecuting(
        ActionExecutingContext context)
    {
        // Before action
    }

    public void OnActionExecuted(
        ActionExecutedContext context)
    {
        // After action
    }
}
```

------------------------------------------------------------------------

## 22. What is DbContext?

### Interview Answer

> `DbContext` is the primary class in Entity Framework Core that
> represents a session with the database. It is responsible for querying
> data, tracking entity changes and saving changes back to the database.
>
> It also exposes `DbSet` properties that represent entities we work
> with.

### Example

``` csharp
public class AppDbContext : DbContext
{
    public DbSet<Employee> Employees { get; set; }

    public AppDbContext(
        DbContextOptions<AppDbContext> options)
        : base(options)
    {
    }
}
```

### Responsibilities

-   Query database
-   Track entities
-   Save changes
-   Manage relationships
-   Coordinate database operations

------------------------------------------------------------------------

## 23. What is Entity Framework?

### Interview Answer

> Entity Framework is an Object-Relational Mapper, or ORM, from
> Microsoft that allows .NET applications to work with databases using
> .NET objects instead of writing SQL for every operation.
>
> Entity Framework Core is the modern, cross-platform version used with
> ASP.NET Core applications.

### Example

Instead of:

``` sql
SELECT *
FROM Employees
WHERE DepartmentId = 10;
```

We can write:

``` csharp
var employees = context.Employees
    .Where(e => e.DepartmentId == 10)
    .ToList();
```

### Benefits

-   LINQ support
-   Change tracking
-   Migrations
-   Relationships
-   Strongly typed queries
-   Less repetitive database code

------------------------------------------------------------------------

# ASP.NET MVC

## 24. Difference Between ViewBag, ViewData and TempData

### Interview Answer

> ViewData and ViewBag are mainly used to pass data from a controller to
> a view during the current request. TempData is designed to persist
> data for the next request, which makes it useful when redirecting from
> one action to another.

  Feature             ViewData     ViewBag           TempData
  ------------------- ------------ ----------------- -------------------
  Type                Dictionary   Dynamic wrapper   Dictionary
  Controller → View   Yes          Yes               Yes
  Survives redirect   No           No                Yes
  Strongly typed      No           No                No
  Common use          View data    View data         Redirect messages

### Example

``` csharp
ViewData["Name"] = "Avi";

ViewBag.Name = "Avi";

TempData["Message"] = "Saved successfully";
```

------------------------------------------------------------------------

# SQL Server

## 25. Difference Between Stored Procedure and Function

### Interview Answer

> A stored procedure is generally used to perform a set of database
> operations, while a function is designed to return a value or table
> and is commonly used inside SQL expressions or queries.
>
> Stored procedures can perform broader procedural operations, while
> functions have more restrictions on side effects.

  -----------------------------------------------------------------------
  Stored Procedure                    Function
  ----------------------------------- -----------------------------------
  Can return result sets              Returns scalar or table

  Supports input/output parameters    Uses parameters but not output
                                      parameters like procedures

  Can perform broader data operations Has restrictions on side effects

  Executed using `EXEC`               Can be used in expressions
                                      depending on function type

  Suitable for workflows              Suitable for reusable
                                      calculations/query logic
  -----------------------------------------------------------------------

### Example

``` sql
EXEC GetEmployees;
```

Function:

``` sql
SELECT dbo.CalculateSalary(1000);
```

------------------------------------------------------------------------

# C

## 26. What is Managed and Unmanaged Code?

### Interview Answer

> Managed code is code executed under the control of the .NET runtime.
> The CLR provides services such as garbage collection, exception
> handling and type safety.
>
> Unmanaged code executes outside the CLR and generally requires more
> direct resource management.

### Managed Code

``` csharp
Employee employee = new Employee();
```

### Unmanaged Code

Native C/C++ code and certain operating-system APIs can involve
unmanaged code.

### Important

> .NET can interact with unmanaged code using mechanisms such as
> P/Invoke and COM interop.

------------------------------------------------------------------------

## 27. What is Boxing and Unboxing?

### Interview Answer

> Boxing is the process of converting a value type into an `object` or
> compatible interface reference. Unboxing is extracting the value type
> from that object.
>
> Boxing can cause an allocation and therefore unnecessary boxing should
> be avoided in performance-sensitive code.

### Boxing

``` csharp
int number = 10;

object obj = number;
```

### Unboxing

``` csharp
int result = (int)obj;
```

### Remember

``` text
Boxing:
Value Type → Object

Unboxing:
Object → Value Type
```

------------------------------------------------------------------------

## 28. What are the Types of Classes in C#?

There is no single official classification called "types of classes",
but common class forms include:

### Concrete Class

Can be instantiated.

``` csharp
public class Employee
{
}
```

### Abstract Class

Cannot be instantiated directly.

``` csharp
public abstract class Employee
{
}
```

### Sealed Class

Cannot be inherited.

``` csharp
public sealed class Employee
{
}
```

### Static Class

Cannot be instantiated or inherited.

``` csharp
public static class Utility
{
}
```

### Partial Class

Allows a class definition to be split across multiple files.

``` csharp
public partial class Employee
{
}
```

### Interview Answer

> Common class forms in C# include concrete, abstract, sealed, static
> and partial classes. Each provides a different design capability.

------------------------------------------------------------------------

## 29. Difference Between Struct and Class

### Interview Answer

> The main difference is that a class is a reference type, while a
> struct is a value type.
>
> Classes are generally suitable for objects with identity and more
> complex behavior, while structs are suitable for small value-like
> types.

  -----------------------------------------------------------------------
  Class                               Struct
  ----------------------------------- -----------------------------------
  Reference type                      Value type

  Supports class inheritance          Cannot inherit from another
                                      class/struct

  Can have null reference             Can be nullable using `?`

  Reference semantics                 Value semantics

  Suitable for complex objects        Suitable for small value-like data
  -----------------------------------------------------------------------

### Example

``` csharp
public class Employee
{
    public string Name { get; set; }
}

public struct Point
{
    public int X;
    public int Y;
}
```

### Important Interview Point

> Avoid saying simply "classes are stored on the heap and structs on the
> stack." That is an oversimplification. Actual storage depends on
> context.

------------------------------------------------------------------------

## 30. What is an Enum?

### Interview Answer

> An enum, or enumeration, is a value type used to represent a fixed set
> of named constants. It improves readability and type safety when a
> variable can have one of a predefined set of values.

### Example

``` csharp
public enum OrderStatus
{
    Pending,
    Confirmed,
    Shipped,
    Delivered
}
```

Usage:

``` csharp
OrderStatus status = OrderStatus.Shipped;
```

By default, enum members use an underlying integral type, commonly
`int`, and start from `0` unless explicit values are assigned.

------------------------------------------------------------------------

## 32. What are Extension Methods?

### Interview Answer

> An extension method allows us to add a method to an existing type
> without modifying the original type or creating a derived type.
>
> Extension methods are defined as static methods inside a static class,
> and the first parameter uses the `this` keyword.

### Example

``` csharp
public static class StringExtensions
{
    public static bool IsEmpty(this string value)
    {
        return string.IsNullOrEmpty(value);
    }
}
```

Usage:

``` csharp
string name = "";

bool result = name.IsEmpty();
```

### Common Example

Many LINQ methods are extension methods:

``` csharp
.Where()
.Select()
.OrderBy()
```

------------------------------------------------------------------------

# ADO.NET

## 31. What is DataSet and DataTable?

### Interview Answer

> A `DataTable` represents one in-memory table containing rows and
> columns. A `DataSet` is an in-memory container that can hold multiple
> DataTables and relationships between them.
>
> They are part of ADO.NET and are useful for working with disconnected
> data.

### Structure

``` text
DataSet
 ├── DataTable 1
 ├── DataTable 2
 └── DataTable 3
```

### Example

``` csharp
DataSet dataSet = new DataSet();

DataTable table = new DataTable("Employees");

table.Columns.Add("Id");
table.Columns.Add("Name");

dataSet.Tables.Add(table);
```

### Remember

``` text
DataTable → One in-memory table

DataSet → Collection of DataTables
```

------------------------------------------------------------------------

# SQL Server

## 33. What are Constraints and Their Types?

### Interview Answer

> Constraints are rules defined on database columns or tables to
> maintain data integrity and ensure that only valid data is stored.

### Types

#### 1. PRIMARY KEY

Uniquely identifies each row.

``` sql
Id INT PRIMARY KEY
```

#### 2. FOREIGN KEY

Maintains relationships between tables.

``` sql
FOREIGN KEY (DepartmentId)
REFERENCES Departments(Id)
```

#### 3. UNIQUE

Prevents duplicate values.

``` sql
Email VARCHAR(100) UNIQUE
```

#### 4. NOT NULL

Prevents NULL values.

``` sql
Name VARCHAR(100) NOT NULL
```

#### 5. CHECK

Enforces a condition.

``` sql
Age INT CHECK (Age >= 18)
```

#### 6. DEFAULT

Provides a default value.

``` sql
Status VARCHAR(20) DEFAULT 'Active'
```

### Purpose

> Constraints maintain data accuracy, consistency and integrity.

------------------------------------------------------------------------

## 34. What is a Recursive Stored Procedure?

### Interview Answer

> A recursive stored procedure is a stored procedure that calls itself,
> directly or indirectly. It can be useful for processing hierarchical
> data such as employee-manager relationships or category trees.
>
> A proper termination condition is required to prevent infinite
> recursion.

### Concept

``` text
Procedure
   ↓
Procedure
   ↓
Procedure
   ↓
Stop condition
```

### Example

``` sql
CREATE PROCEDURE GetHierarchy
    @EmployeeId INT
AS
BEGIN
    -- Process current employee

    -- Find child employee

    -- Call procedure for child
END;
```

### Interview Point

> For hierarchical queries, a recursive CTE is often a more appropriate
> SQL Server solution than recursive stored procedure calls.

------------------------------------------------------------------------

# Security

## 35. Difference Between Authentication and Authorization

### Interview Answer

> Authentication determines **who you are**, while authorization
> determines **what you are allowed to do**.
>
> For example, when a user logs in, the system authenticates the user's
> identity. After authentication, authorization checks whether that user
> has permission to access a particular resource or perform an
> operation.

### Example

``` text
Authentication
"Who are you?"
       ↓
User = Avi

Authorization
"What can you do?"
       ↓
Admin → Can delete users
User  → Cannot delete users
```

### In ASP.NET Core

``` csharp
[Authorize]
public IActionResult DeleteUser(int id)
{
    // ...
}
```

Authorization can be based on:

-   Roles
-   Claims
-   Policies
-   Permissions

### Easy Way to Remember

> **Authentication = Identity**
>
> **Authorization = Permission**

------------------------------------------------------------------------

# Quick Revision

  ---------------------------------------------------------------------------
  \#                      Topic                       Key Point
  ----------------------- --------------------------- -----------------------
  1                       C#                          Strongly typed,
                                                      object-oriented .NET
                                                      language

  2                       CLR                         Runtime that executes
                                                      and manages .NET code

  3                       Garbage Collection          Automatically reclaims
                                                      unreachable managed
                                                      objects

  4                       Stored Procedure            Reusable SQL program
                                                      stored in SQL Server

  5                       Indexing                    Improves data retrieval
                                                      performance

  6                       View                        Virtual database object
                                                      based on a query

  7                       Trigger                     Automatically executes
                                                      on database events

  8                       AJAX                        Asynchronous
                                                      client-server
                                                      communication

  9                       `document.ready()`          Runs when DOM is ready

  10                      Aggregate Functions         Calculate values across
                                                      multiple rows

  11                      Output Parameter            Returns a value through
                                                      a procedure parameter

  12                      Pure/Impure Pipe            Pure depends on input
                                                      changes; impure runs
                                                      during change detection

  13                      Component/Module            UI building block /
                                                      organization mechanism

  14                      Middleware                  Component in ASP.NET
                                                      Core request pipeline

  15                      DI                          Dependencies supplied
                                                      externally

  16                      Authentication              Verifies identity

  17                      Filters                     Execute logic around
                                                      MVC/API processing

  18                      jQuery                      JavaScript
                                                      utility/library

  19                      Two-Way Binding             UI ↔ component data
                                                      synchronization

  20                      Decorators                  Provide
                                                      Angular/TypeScript
                                                      metadata

  21                      Directives                  Add behavior/change DOM

  22                      DbContext                   EF Core database
                                                      session/unit of work

  23                      Entity Framework            ORM for .NET
                                                      applications

  24                      ViewBag/ViewData/TempData   MVC data-passing
                                                      mechanisms

  25                      SP vs Function              Procedure performs
                                                      operations; function
                                                      returns value/table

  26                      Managed/Unmanaged           CLR-controlled vs
                                                      outside CLR

  27                      Boxing/Unboxing             Value type ↔ object
                                                      conversion

  28                      Class Types                 Concrete, abstract,
                                                      sealed, static, partial

  29                      Struct vs Class             Value type vs reference
                                                      type

  30                      Enum                        Named set of constants

  31                      DataSet/DataTable           Collection of tables vs
                                                      one table

  32                      Extension Methods           Add methods to existing
                                                      types

  33                      Constraints                 Enforce database
                                                      integrity

  34                      Recursive SP                Procedure that calls
                                                      itself

  35                      Auth vs Authorization       Identity vs permission
  ---------------------------------------------------------------------------

------------------------------------------------------------------------

# Interview Answering Rule

For each question, follow this structure:

``` text
1. Definition
2. How it works
3. Simple example
4. Real-world use case
5. Advantages / limitations
```

Do **not** memorize every sentence word-for-word. Understand the concept
and speak naturally.
