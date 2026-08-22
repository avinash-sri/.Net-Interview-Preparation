# First CRUD Flow

## Request Flow

```text
HTTP Request
     ↓
Controller
     ↓
Application Service
     ↓
Repository Interface
     ↓
Repository Implementation
     ↓
DbContext
     ↓
PostgreSQL
```

## Controller Responsibility

The controller handles HTTP concerns:

- Request
- Response
- HTTP status codes
- Routing

It should not contain database logic or core business rules.

## Application Service

The Application Service contains application/use-case logic.

Example:

```csharp
public async Task<TaskResponse> CreateAsync(
    CreateTaskRequest request)
{
    var task = new TaskItem(request.Title);

    await _repository.AddAsync(task);
    await _repository.SaveChangesAsync();

    return new TaskResponse
    {
        Id = task.Id,
        Title = task.Title,
        IsCompleted = task.IsCompleted
    };
}
```

## DTO

DTO stands for Data Transfer Object.

DTOs define the data that crosses application boundaries.

Examples:

```text
CreateTaskRequest
TaskResponse
```

DTOs prevent domain entities from becoming the API contract.

## Dependency Direction

```text
API
 ↓
Application
 ↓
Domain

Infrastructure
 ↓
Application
 ↓
Domain
```

Application depends on repository abstractions.

Infrastructure provides the implementations.

## Why shouldn't Controllers directly use DbContext?

Directly using DbContext inside controllers couples the API layer to persistence technology and mixes HTTP concerns with database/application logic.

Keeping database access in Infrastructure provides better separation of concerns and makes the architecture easier to maintain and test.

## `new` and Tight Coupling

Using `new` is not automatically tight coupling.

Creating a Domain entity:

```csharp
var task = new TaskItem(request.Title);
```

is appropriate because the application needs to construct the business entity.

The problem is directly creating replaceable infrastructure dependencies:

```csharp
var repository = new SqlTaskRepository();
```

inside a service.

---

# Interview Questions

### What is DTO?

A DTO is an object used to transfer data between application boundaries without exposing the internal domain model.

### Why shouldn't controllers contain business logic?

Controllers should focus on HTTP concerns. Keeping business logic in the Application/Domain layers improves separation of concerns, testability, and maintainability.

### Why shouldn't Application depend directly on EF Core?

Because EF Core is an infrastructure/persistence detail. Application should depend on abstractions rather than a specific persistence technology.

### Explain the complete request flow.

A request reaches the Controller, which calls the Application Service. The Application Service uses an abstraction such as `ITaskRepository`. Infrastructure provides the repository implementation using EF Core and `DbContext`, which ultimately persists data to PostgreSQL.