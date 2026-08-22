# EF Core Projection

## What is Projection?

Projection means selecting only the data that we need.

Use `Select()` for projection.

Example:

```csharp
var result = await _dbContext.Tasks
    .Select(x => new TaskResponse
    {
        Id = x.Id,
        Title = x.Title,
        IsCompleted = x.IsCompleted
    })
    .ToListAsync();
```

EF Core can translate this into SQL that selects only the required columns.

## Why Use Projection?

Projection can reduce:

- Data transferred from database
- Memory usage
- Unnecessary object creation
- Application processing

## Projection to DTO

Instead of returning EF entities directly:

```csharp
return Ok(tasks);
```

use a DTO:

```csharp
public class TaskResponse
{
    public int Id { get; set; }
    public string Title { get; set; }
    public bool IsCompleted { get; set; }
}
```

Then:

```csharp
var result = await _dbContext.Tasks
    .Select(x => new TaskResponse
    {
        Id = x.Id,
        Title = x.Title,
        IsCompleted = x.IsCompleted
    })
    .ToListAsync();
```

## Combining LINQ Operations

```csharp
var result = await _dbContext.Tasks
    .Where(x => x.IsCompleted)
    .Select(x => new TaskResponse
    {
        Id = x.Id,
        Title = x.Title
    })
    .ToListAsync();
```

`Where()`:

> Which records do I need?

`Select()`:

> Which data do I need from those records?

`ToListAsync()`:

> Execute the query.

## Important

Prefer projection before `ToListAsync()` when querying a database.

Avoid loading unnecessary columns and then filtering/mapping in memory.