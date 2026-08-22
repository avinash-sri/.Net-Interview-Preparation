# Concurrency & Thread Safety

## Concurrency

Concurrency means multiple requests/operations can happen around the same time.

## Race Condition

A race condition happens when multiple requests access or modify the same data at the same time and the result becomes incorrect.

Example:

```csharp
counter++;
```

Two requests can both read the same value before either one saves the new value.

## Thread Safety

Thread-safe code works correctly when multiple threads access it at the same time.

## lock

`lock` allows only one thread at a time to execute a particular section of code.

```csharp
lock (_lock)
{
    counter++;
}
```

Use `lock` for small synchronous critical sections.

## Interlocked

Useful for simple thread-safe operations.

```csharp
Interlocked.Increment(ref counter);
```

## Concurrent Collections

Collections designed for concurrent access:

```text
ConcurrentDictionary
ConcurrentQueue
ConcurrentBag
```

## SemaphoreSlim

Controls how many operations can execute at the same time.

```csharp
private readonly SemaphoreSlim _semaphore = new(3);
```

This allows up to 3 operations at a time.

## lock vs SemaphoreSlim

`lock`:

- One thread at a time
- Good for small synchronous operations

`SemaphoreSlim`:

- Works with async code
- Can allow multiple operations at the same time

## Distributed Applications

A C# `lock` only works inside one application instance.

If we have:

```text
Server 1
Server 2
Server 3
```

each server has its own memory and locks.

For distributed concurrency, we may need:

- Database transactions
- Optimistic concurrency
- Pessimistic locking
- Distributed locks

## Optimistic Concurrency

Assume conflicts are uncommon and detect conflicts when saving.

## Pessimistic Concurrency

Lock the data so other operations have to wait.