# Deadlocks

## What is a Deadlock?

A deadlock happens when two or more operations wait for each other and none of them can continue.

Example:

```text
Thread A → has Lock 1 → wants Lock 2
Thread B → has Lock 2 → wants Lock 1
```

Both wait forever.

## Simple Example

```csharp
lock (lock1)
{
    lock (lock2)
    {
        // work
    }
}
```

Another part of the application should acquire locks in the same order.

Avoid:

```text
Lock 1 → Lock 2
```

in one place and:

```text
Lock 2 → Lock 1
```

in another place.

## Deadlock vs Race Condition

Race condition:

```text
Multiple operations
      ↓
Shared data
      ↓
Wrong result
```

Deadlock:

```text
Operation A → waits for B
Operation B → waits for A
      ↓
Stuck
```

## Result / Wait

Avoid:

```csharp
var result = SomeAsyncMethod().Result;

SomeAsyncMethod().Wait();
```

Prefer:

```csharp
var result = await SomeAsyncMethod();
```

`Result` and `Wait()` can cause deadlocks in some environments and can block ThreadPool threads, reducing scalability.

## ASP.NET Core

Modern ASP.NET Core does not have the same classic SynchronizationContext deadlock behavior found in older ASP.NET.

However, blocking async operations is still discouraged because it blocks ThreadPool threads.

## Async Synchronization

Don't use synchronous `lock` for asynchronous waiting.

Use:

```csharp
await semaphore.WaitAsync();

try
{
    await SomeOperationAsync();
}
finally
{
    semaphore.Release();
}
```

## Prevention

- Acquire locks in a consistent order
- Keep critical sections small
- Avoid unnecessary locks
- Prefer async APIs
- Avoid `.Result` and `.Wait()`
- Use `SemaphoreSlim` when async synchronization is required