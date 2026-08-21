# Entity Framework Core

## What is EF Core?

Entity Framework Core is Microsoft's ORM for .NET that maps database tables to C# objects.

---

# ORM

Object Relational Mapper

Maps:

Database Table

↓

C# Class

---

# DbContext

DbContext manages communication with the database.

Responsibilities:

- Database session
- Change Tracking
- Query execution
- SaveChanges
- Transactions

---

# DbSet

Represents a collection of entities.

Examples:

```csharp
context.Users

context.Tasks
```

---

# Change Tracker

Tracks:

- Added
- Modified
- Deleted
- Unchanged

No SQL executes until SaveChanges().

---

# SaveChanges()

Generates SQL based on tracked changes and sends it to the database.

---

# Why is DbContext Scoped?

One HTTP request should share one DbContext instance so all operations use the same Change Tracker.

---

# Performance Tips

- Avoid SaveChanges inside loops.
- Use AsNoTracking for read-only queries.
- Filter and paginate large queries.
- Keep DbContext short-lived.

---

# Interview Questions

### What is EF Core?

EF Core is Microsoft's ORM that allows developers to work with relational databases using C# objects.

### What is DbContext?

DbContext manages communication with the database, change tracking, and persistence.

### What is DbSet?

Represents a collection of entities and provides query/update operations.

### When does EF Core execute SQL?

When SaveChanges() or query execution occurs.

### What is Change Tracking?

A mechanism that tracks entity state changes so EF Core knows what SQL to generate during SaveChanges().


# EF Core + Infrastructure

## Infrastructure Layer

Infrastructure contains implementation details that interact with external technologies.

Examples:

- EF Core
- PostgreSQL
- Redis
- RabbitMQ
- File system
- External APIs

## AppDbContext

`DbContext` is the main EF Core abstraction used to query and persist entities.

It provides:

- Change tracking
- Query execution
- SaveChanges
- Transaction support
- Database interaction

## DbSet

`DbSet<T>` represents a collection of entities of type `T` and provides query/update operations.

Example:

```csharp
public DbSet<TaskItem> Tasks => Set<TaskItem>();
```

## Entity Configuration

Database-specific mapping should remain outside the Domain.

Example:

```csharp
builder.Property(x => x.Title)
    .IsRequired()
    .HasMaxLength(200);
```

The Domain says:

> Task has a Title.

Infrastructure says:

> Title is required and has a maximum length of 200 in the persistence model.

## Repository Implementation

The repository interface is defined by Application:

```text
Application
    ↓
ITaskRepository
```

Infrastructure implements it:

```text
Infrastructure
    ↓
TaskRepository
```

This allows Application to depend on an abstraction rather than EF Core.

## Migration

A migration describes database schema changes.

```bash
dotnet ef migrations add InitialCreate
```

## Database Update

Applies migrations to the database.

```bash
dotnet ef database update
```

### Important

Migration creation and database update are different operations.

---

## Interview Questions

### Why does AppDbContext belong in Infrastructure?

Because it depends on EF Core and represents persistence details. The Domain should remain independent of databases and frameworks.

### Why does TaskRepository belong in Infrastructure?

Because it contains the actual persistence implementation using EF Core.

### Why is ITaskRepository in Application?

Application defines the abstraction it needs, while Infrastructure provides the implementation.

### What is the difference between a migration and database update?

A migration describes schema changes. Database update applies those changes to the database.