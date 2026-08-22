# Database Isolation Levels

## Isolation

Isolation controls how transactions interact with each other and what data one transaction can see from another transaction.

Common isolation levels:

```text
Read Uncommitted
Read Committed
Repeatable Read
Serializable
```

From lower to higher isolation:

```text
Read Uncommitted
       ↓
Read Committed
       ↓
Repeatable Read
       ↓
Serializable
```

Higher isolation generally provides stronger consistency but can reduce concurrency.

## Read Uncommitted

Transactions can potentially read uncommitted changes from other transactions.

Problem:

```text
Dirty Read
```

## Read Committed

A transaction only reads committed data.

Uncommitted changes from other transactions are not visible.

## Repeatable Read

If a transaction reads a row, another transaction cannot change that row in a way that causes the first transaction to see a different value during the transaction.

## Serializable

Strongest standard isolation level.

Concurrent transactions behave approximately as if they were executed one after another.

It provides strong consistency but can reduce concurrency and increase blocking.

## Dirty Read

Example:

```text
Transaction A → changes data
Transaction B → reads it
Transaction A → rollback
```

Transaction B read data that was never committed.

## EF Core

Example:

```csharp
await using var transaction =
    await dbContext.Database.BeginTransactionAsync(
        IsolationLevel.ReadCommitted);
```

## Important

Higher isolation is not always better.

Choose an isolation level based on:

- Consistency requirements
- Concurrency requirements
- Performance