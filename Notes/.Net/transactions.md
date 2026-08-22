# Database Transactions

## What is a Transaction?

A transaction groups multiple database operations into one unit.

Either:

- All operations succeed
- Or all operations are rolled back

## Commit

`COMMIT` permanently saves the changes.

## Rollback

`ROLLBACK` undoes changes made during the transaction.

## EF Core Example

```csharp
await using var transaction =
    await dbContext.Database.BeginTransactionAsync();

try
{
    // Operation 1
    // Operation 2

    await transaction.CommitAsync();
}
catch
{
    await transaction.RollbackAsync();
    throw;
}
```

## ACID

### Atomicity

All operations succeed or all fail.

### Consistency

Database remains in a valid state.

### Isolation

Concurrent transactions should not incorrectly interfere with each other.

### Durability

Committed changes remain persisted even after failures.

## Transaction vs lock

C# `lock`:

```text
Protects application memory
```

Database transaction:

```text
Protects/coordinates database operations
```

They solve different problems.

## When to Use Transactions

Use a transaction when multiple database operations need to succeed or fail together.

Example:

```text
Create Order
+
Create Order Items
+
Update Inventory
```

## Important

Don't manually create a transaction for every database operation.

A single database statement is generally already atomic, and EF Core handles transaction behavior around `SaveChanges()`.

Manual transactions are useful when multiple operations must be treated as one unit.