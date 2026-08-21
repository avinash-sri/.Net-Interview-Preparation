# ASP.NET Core Middleware

## Definition

Middleware is a component in the ASP.NET Core request pipeline that can inspect, modify, process, or short-circuit HTTP requests and responses.

## Request Pipeline

```text
Request
   ↓
Middleware A
   ↓
Middleware B
   ↓
Controller
   ↓
Response
   ↑
Middleware B
   ↑
Middleware A
```

## `next`

`next` represents the next middleware in the pipeline.

```csharp
await next(context);
```

continues execution.

Code before `next()` executes during the request path.

Code after `next()` executes during the response path.

## Middleware Ordering

If middleware is registered:

```text
A → B → C
```

Request:

```text
A Before
B Before
C Before
Controller
```

Response:

```text
C After
B After
A After
```

## Short-Circuiting

Middleware can stop the pipeline by not calling `next()`.

```csharp
if (!authorized)
{
    context.Response.StatusCode = 401;
    return;
}
```

## `Use`

Can execute code and continue the pipeline.

```csharp
app.Use(...);
```

## `Run`

Terminates the pipeline.

```csharp
app.Run(...);
```

## `Map`

Creates a branch in the pipeline.

```csharp
app.Map("/health", ...);
```

## Authentication vs Authorization

Authentication:

> Who are you?

Authorization:

> Are you allowed to perform this operation?

Typical order:

```csharp
app.UseAuthentication();
app.UseAuthorization();
app.MapControllers();
```

## Middleware vs Filters

Middleware operates at the HTTP pipeline level.

Filters operate closer to MVC/controller actions.

## Common Middleware Uses

- Exception handling
- Logging
- Authentication
- Authorization
- CORS
- Rate limiting
- Request/response processing
- Correlation IDs

## Interview Question

### Why does middleware order matter?

Because middleware executes in the order it is registered for the request and in reverse order for the response. Some middleware depends on other middleware having already executed.

### What happens if middleware doesn't call `next()`?

The pipeline is short-circuited and downstream middleware/controllers aren't executed.