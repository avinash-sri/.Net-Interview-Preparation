# Optimistic Concurrency in EF Core

## Problem

Two users can read the same record and try to update it at the same time.

Example:

```text
User A → reads Version 5
User B → reads Version 5

User A → updates → Version 6

User B → tries to update Version 5
```

The second update should fail because the record has changed.

## Optimistic Concurrency

Optimistic concurrency assumes that conflicts are uncommon.

Instead of locking the record:

```text
Read
 ↓
Work
 ↓
Save
 ↓
Check if record changed
```

If the record hasn't changed:

```text
Success
```

If it changed:

```text
Concurrency conflict
```

## Concurrency Token

EF Core can use a property as a concurrency token.

Example:

```csharp
public int Version { get; set; }
```

Configuration:

```csharp
builder.Property(x => x.Version)
       .IsConcurrencyToken();
```

EF Core uses the original value when updating.

Conceptually:

```sql
UPDATE Tasks
SET Title = @title,
    Version = 6
WHERE Id = 1
AND Version = 5;
```

If the version changed:

```text
Rows affected = 0
```

EF Core throws:

```csharp
DbUpdateConcurrencyException
```

## HTTP Response

A concurrency conflict can commonly be returned as:

```text
409 Conflict
```

because the request conflicts with the current state of the resource.

## Optimistic vs Pessimistic

Optimistic:

```text
Don't lock
 ↓
Detect conflict while saving
```

Pessimistic:

```text
Lock
 ↓
Other operations wait
```

## C# lock vs Database Concurrency

C# `lock`:

```text
Protects in-memory data
```

Optimistic concurrency:

```text
Protects database updates from overwriting newer changes
```

# Optimistic Concurrency Implementation

## Goal

Prevent one user from accidentally overwriting another user's changes.

Example:

```text
User A → reads Version 1
User B → reads Version 1

User A → updates → Version 2

User B → updates using Version 1
       ↓
Concurrency Conflict
```

## Entity

```csharp
public int Version { get; private set; } = 1;
```

## EF Configuration

```csharp
builder.Property(x => x.Version)
    .IsConcurrencyToken();
```

This tells EF Core to use `Version` when checking for concurrent updates.

## Update

Conceptually EF executes:

```sql
UPDATE Tasks
SET
    Title = @title,
    Version = 2
WHERE
    Id = @id
    AND Version = 1;
```

If the database still contains Version 1:

```text
Rows affected = 1
→ Success
```

If another user already changed it to Version 2:

```text
Rows affected = 0
→ Concurrency Conflict
```

EF Core throws:

```csharp
DbUpdateConcurrencyException
```

## HTTP Response

A concurrency conflict can be mapped to:

```text
409 Conflict
```

## Important

`IsConcurrencyToken()` does not automatically increment an integer version.

The application must update the version value or use a database-generated concurrency mechanism.