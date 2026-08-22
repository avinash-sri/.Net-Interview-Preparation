# Async/Await in .NET

## async

`async` allows a method to use `await` and participate in asynchronous execution.

Important:

> `async` does not automatically create a new thread.

## await

`await` asynchronously waits for an operation to complete without blocking the current thread while an asynchronous operation is pending.

Example:

```csharp
var result = await repository.GetAllAsync();
```

## Task

A `Task` represents an asynchronous operation that may complete in the future.

```csharp
Task
Task<T>
```

`Task`:

> Asynchronous operation with no result.

`Task<T>`:

> Asynchronous operation that produces a result.

## Task vs Thread

Task:

> Represents an asynchronous operation.

Thread:

> Execution resource that executes code.

A Task does not necessarily mean a new thread is created.

## I/O-Bound Operations

Examples:

- Database calls
- HTTP calls
- File operations
- Network operations

Prefer asynchronous APIs:

```csharp
await db.SaveChangesAsync();
await httpClient.GetAsync(url);
```

## CPU-Bound Operations

Examples:

- Complex calculations
- Image processing
- Compression
- CPU-intensive algorithms

These may require CPU parallelism or `Task.Run` depending on the scenario.

## Task.Run

Do not wrap naturally asynchronous I/O in `Task.Run`.

Bad:

```csharp
await Task.Run(() => db.Tasks.ToList());
```

Better:

```csharp
await db.Tasks.ToListAsync();
```

## Async All the Way

Prefer:

```text
Controller
   ↓ await
Service
   ↓ await
Repository
   ↓ await
EF Core
   ↓
Database
```

Avoid mixing async APIs with `.Result` or `.GetAwaiter().GetResult()`.

## async void

Avoid `async void` except for scenarios such as event handlers.

Prefer:

```csharp
async Task
```

or:

```csharp
async Task<T>
```

## Important

Async does not necessarily make an individual operation faster.

It allows the application to use threads more efficiently while waiting for asynchronous I/O.