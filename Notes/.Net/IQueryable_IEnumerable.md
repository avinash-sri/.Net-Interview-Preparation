# IQueryable vs IEnumerable

## IQueryable

`IQueryable` is commonly used by EF Core to build database queries.

Example:

```csharp
IQueryable<TaskItem> query =
    _dbContext.Tasks;

query = query.Where(x => x.IsCompleted);
```

The query can be translated into SQL.

Example:

```sql
SELECT *
FROM Tasks
WHERE IsCompleted = true;
```

The filtering happens in the database.

## IEnumerable

`IEnumerable` is used to work with data that is already available in memory.

Example:

```csharp
var tasks = await _dbContext.Tasks.ToListAsync();

var completed = tasks
    .Where(x => x.IsCompleted);
```

The filtering happens in C# memory.

## Important Difference

### IQueryable

```text
Build query
    ↓
Database executes query
    ↓
Return required data
```

### IEnumerable

```text
Data already loaded
    ↓
C# filters data
```

## Deferred Execution

An `IQueryable` query is generally not executed when `Where()` is called.

Example:

```csharp
var query = _dbContext.Tasks
    .Where(x => x.IsCompleted);
```

The query is executed when a terminal operation is called:

```csharp
await query.ToListAsync();
```

Common terminal operations:

```text
ToList
ToListAsync
First
FirstAsync
FirstOrDefault
FirstOrDefaultAsync
Single
SingleAsync
Count
CountAsync
Any
AnyAsync
```

## Performance

Avoid:

```csharp
var tasks = await _dbContext.Tasks.ToListAsync();

var completed = tasks
    .Where(x => x.IsCompleted);
```

when the database contains a large amount of data.

Prefer:

```csharp
var completed = await _dbContext.Tasks
    .Where(x => x.IsCompleted)
    .ToListAsync();
```

This allows the database to perform the filtering.

## Important

`IEnumerable` is not bad.

Use it when the data is already in memory.

Use `IQueryable` when you want to continue building a database query.