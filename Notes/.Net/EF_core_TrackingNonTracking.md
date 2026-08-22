# EF Core Tracking vs AsNoTracking

## Tracking

By default, EF Core tracks entities returned from queries.

Example:

```csharp
var task = await _dbContext.Tasks
    .FirstAsync(x => x.Id == id);

task.Title = "New Title";

await _dbContext.SaveChangesAsync();
```

EF Core detects the change because the entity is being tracked.

## AsNoTracking

Use `AsNoTracking()` when the data is read-only.

```csharp
var tasks = await _dbContext.Tasks
    .AsNoTracking()
    .ToListAsync();
```

EF Core doesn't keep tracking information for these entities.

## Why Use AsNoTracking?

For read-only queries:

- Less tracking overhead
- Less memory usage
- Can improve query performance

## When to Use

### Read-only

```csharp
var tasks = await _dbContext.Tasks
    .AsNoTracking()
    .ToListAsync();
```

### Read + Modify

Normal tracking can be useful:

```csharp
var task = await _dbContext.Tasks
    .FirstAsync(x => x.Id == id);

task.Title = "New Title";

await _dbContext.SaveChangesAsync();
```

## Important

Don't use `AsNoTracking()` everywhere.

Use it when EF Core doesn't need to track the returned entities.

## Projection

For APIs, we can also select only the required fields:

```csharp
var tasks = await _dbContext.Tasks
    .Select(x => new TaskResponse
    {
        Id = x.Id,
        Title = x.Title
    })
    .ToListAsync();
```

This allows the database to return only the required columns.