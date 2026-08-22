# ThreadPool, Task.Run & Concurrency

## ThreadPool

The .NET ThreadPool is a managed pool of reusable threads used to execute work without creating a new thread for every operation.

Benefits:

- Thread reuse
- Lower creation overhead
- Better scalability
- Efficient resource management

## async/await and ThreadPool

When an asynchronous I/O operation reaches `await`, the thread does not need to remain blocked while waiting.

```text
Request
  ↓
ThreadPool Thread
  ↓
Start async I/O
  ↓
await
  ↓
Thread becomes available
  ↓
I/O completes
  ↓
Continuation executes on a ThreadPool thread
```

The continuation does not necessarily run on the same physical thread.

## Task.Run

`Task.Run` schedules synchronous work on a ThreadPool thread.

Example:

```csharp
await Task.Run(() => ExpensiveCalculation());
```

It can be useful for appropriate CPU-bound work.

## Don't Wrap Async I/O in Task.Run

Bad:

```csharp
await Task.Run(() =>
    dbContext.Tasks.ToList());
```

Better:

```csharp
await dbContext.Tasks.ToListAsync();
```

The second approach uses EF Core's asynchronous database API directly.

## ThreadPool Starvation

ThreadPool starvation occurs when available ThreadPool threads are occupied or blocked for long periods, preventing new work from being processed efficiently.

Blocking async operations can contribute:

```csharp
.GetAwaiter().GetResult()
.Result
.Wait()
```

Prefer:

```csharp
await SomeAsyncOperation();
```

## Concurrency

Multiple operations are in progress during overlapping periods.

## Parallelism

Multiple operations execute simultaneously, typically using multiple CPU cores.

## Key Difference

```text
Concurrency:
Multiple things are being handled.

Parallelism:
Multiple things are executing at the same time.
```