# EF Core Include and N+1 Problem

## Include

`Include()` is used to load related entities.

Example:

```csharp
var tasks = await _dbContext.Tasks
    .Include(x => x.Comments)
    .ToListAsync();
```

This loads Tasks along with their Comments.

## ThenInclude

Used when loading a relationship from an already included relationship.

Example:

```csharp
var tasks = await _dbContext.Tasks
    .Include(x => x.Comments)
    .ThenInclude(x => x.User)
    .ToListAsync();
```

Relationship:

```text
Task
 ↓
Comments
 ↓
User
```

## N+1 Query Problem

Suppose we load 100 tasks:

```text
1 query → Tasks
```

Then execute another query for each task's comments:

```text
100 queries → Comments
```

Total:

```text
101 queries
```

This is the N+1 problem.

It can cause poor performance.

## Include vs Projection

Use `Include()` when you actually need related entities.

```csharp
.Include(x => x.Comments)
```

Use projection when you only need specific fields.

```csharp
.Select(x => new TaskResponse
{
    Id = x.Id,
    Title = x.Title
})
```

Projection can avoid loading unnecessary data.

## Important

Don't add `Include()` for every relationship.

Only load the data required by the API.