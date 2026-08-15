# SOLID Principles

SOLID is a set of five object-oriented design principles that help developers build maintainable, scalable, loosely coupled, and testable applications.

---

# S - Single Responsibility Principle (SRP)

## Definition

A class should have only one reason to change.

## Why?

A class should focus on a single responsibility. If a class performs multiple unrelated tasks, a change in one responsibility may unintentionally affect another.

## Bad Example

```csharp
public class UserService
{
    public void Register(User user) { }

    public void SendEmail(User user) { }

    public void GenerateReport() { }

    public void LogActivity() { }
}
```

### Why is this bad?

This class has multiple responsibilities:

- User registration
- Email sending
- Report generation
- Logging

Each responsibility changes for different reasons, making the class difficult to maintain and test.

## Good Example

```text
UserRegistrationService

EmailService

ReportService

LoggingService
```

Each class has one responsibility.

---

# O - Open Closed Principle (OCP)

## Definition

Software entities should be open for extension but closed for modification.

## Why?

New functionality should be added without modifying existing working code.

## Bad Example

```csharp
if(paymentType == "CreditCard")
{
}

else if(paymentType == "UPI")
{
}

else if(paymentType == "PayPal")
{
}
```

### Why is this bad?

Every time a new payment method is introduced, this code must be modified.

This increases the risk of introducing bugs into existing functionality.

## Good Example

```text
IPaymentGateway

↑

CreditCardPayment

UPIPayment

PayPalPayment
```

To add Stripe:

```text
StripePayment
```

No existing code changes.

---

# L - Liskov Substitution Principle (LSP)

## Definition

Derived classes should be replaceable for their base classes without affecting program correctness.

## Bad Example

```text
Bird

↓

Penguin
```

Bird exposes:

```csharp
Fly()
```

Penguin cannot fly.

### Why is this bad?

If client code expects every Bird to fly, replacing a Bird with a Penguin breaks the program.

This violates LSP.

## Good Example

```text
Bird

↓

FlyingBird

↓

Sparrow

Eagle

Bird

↓

NonFlyingBird

↓

Penguin
```

Now every derived class satisfies the expected behavior.

---

# I - Interface Segregation Principle (ISP)

## Definition

Clients should not be forced to depend on methods they do not use.

## Bad Example

```csharp
public interface IWorker
{
    Work();

    Eat();

    Sleep();

    AttendMeeting();
}
```

Robot implements IWorker.

### Why is this bad?

A Robot does not eat or sleep.

It is forced to implement unnecessary methods.

## Good Example

```text
IWork

IEat

ISleep

IMeeting
```

Each class implements only the interfaces it needs.

---

# D - Dependency Inversion Principle (DIP)

## Definition

High-level modules should not depend on low-level modules.

Both should depend on abstractions.

## Bad Example

```text
TaskService

↓

SqlTaskRepository
```

### Why is this bad?

TaskService is tightly coupled to SQL Server.

Changing the database requires modifying business logic.

## Good Example

```text
TaskService

↓

ITaskRepository

↑

SqlTaskRepository
```

Now TaskService depends only on the abstraction.

Database implementations can change without modifying TaskService.

---

# Interview Questions

## What is SOLID?

SOLID is a set of five object-oriented design principles that improve maintainability, scalability, testability, and loose coupling.

---

## Explain SRP.

A class should have only one reason to change.

---

## Explain OCP.

Software should be open for extension but closed for modification.

---

## Explain LSP.

Derived classes should be replaceable with their base classes without changing program behavior.

---

## Explain ISP.

Interfaces should be small and focused so clients implement only what they need.

---

## Explain DIP.

High-level and low-level modules should both depend on abstractions rather than concrete implementations.

---

# Production Tips

- Don't create interfaces for every class.
- Don't over-engineer small applications.
- Follow SOLID where it improves maintainability.
- Use engineering judgment instead of applying principles blindly.