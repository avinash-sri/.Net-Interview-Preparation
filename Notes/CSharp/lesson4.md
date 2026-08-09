# Dependency Injection (DI)

## Definition

Dependency Injection is a design pattern where a class receives its dependencies from an external source instead of creating them itself.

## What is a Dependency?

A dependency is any object or service that another class requires to perform its work.

## Why do we use DI?

- Loose coupling
- Easier testing
- Better maintainability
- Flexible implementations
- Separation of responsibilities

## What is IoC?

Inversion of Control is a design principle where the responsibility of creating and managing objects is transferred from the class to an external component, such as a DI container.

Dependency Injection is one way to implement IoC.

## Interview Questions

### What is Dependency Injection?

Dependency Injection is a design pattern where dependencies are supplied to a class from an external source instead of being created by the class itself.

### What is a dependency?

A dependency is an object or service that another class requires to perform its work.

### What is the difference between IoC and DI?

IoC is the principle of transferring control of object creation to an external component. Dependency Injection is one technique used to achieve IoC.    

# Dependency Injection Lifetimes

## Transient

Creates a new instance every time the service is requested.

Use Cases:
- Validators
- Helpers
- Lightweight stateless services

---

## Scoped

Creates one instance per HTTP request.

Use Cases:
- DbContext
- Repository
- Business Services

---

## Singleton

Creates one instance for the entire application lifetime.

Use Cases:
- Configuration
- Cache
- Feature Flags

---

## Interview Questions

### Difference between Transient, Scoped and Singleton?

Transient creates a new instance every time.

Scoped creates one instance per request.

Singleton creates one instance for the lifetime of the application.

### Why is DbContext Scoped?

Because all operations in a single HTTP request should use the same DbContext instance, enabling consistent tracking and transaction behavior within that request.

# Domain Layer

## Responsibility

Contains the core business rules and business entities.

## Contains

- Entities
- Business Rules
- Value Objects (later)
- Domain Exceptions

## Does NOT Contain

- Controllers
- Database Logic
- EF Core
- Redis
- HTTP
- AWS

---

# Application Layer

## Responsibility

Contains the application's use cases.

## Contains

- Interfaces
- Services
- Commands
- Queries

## Depends On

Domain

---

## Interview Question

Why are repository interfaces placed in the Application layer?

Because the Application layer depends on abstractions to perform use cases, while the Infrastructure layer provides the implementations.


# Interface

## Definition

An interface is a contract that defines what operations a class must provide without defining how they are implemented.

## Advantages

- Loose coupling
- Multiple implementations
- Better testability
- Supports Dependency Injection
- Follows the Dependency Inversion Principle

---

# Dependency Inversion Principle (DIP)

## Definition

High-level modules should not depend on low-level modules. Both should depend on abstractions.

Abstractions should not depend on details. Details should depend on abstractions.

---

## Example

TaskService
      ↓
ITaskRepository
      ↑
SqlTaskRepository

---

## Interview Question

### Difference between Dependency Injection and Dependency Inversion Principle

Dependency Inversion Principle is a design principle that encourages depending on abstractions instead of concrete implementations.

Dependency Injection is a design pattern used to provide those abstractions to a class.