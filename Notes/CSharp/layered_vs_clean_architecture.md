# Layered Architecture vs Clean Architecture

## Layered Architecture

Presentation -> Business -> Data -> Database

Business layer often depends on the data access implementation.

## Clean Architecture

API -> Application -> Domain

Infrastructure -> Domain

## Dependency Rule

Dependencies always point toward the Domain.

The Domain should not depend on infrastructure or external frameworks.

## Advantages

- Loose coupling
- Better testability
- Better maintainability
- Easy to replace external technologies

## Disadvantages

- More projects
- More abstraction
- Increased complexity for small applications

## Interview Question

### Why is the Domain layer at the center?

Because it contains the core business rules of the application and should remain independent of frameworks, databases, UI, and external services.

### Why does Infrastructure depend on Domain?

Infrastructure implements the contracts and works with the Domain entities. It needs to know about the business model, but the business model should not know about infrastructure.