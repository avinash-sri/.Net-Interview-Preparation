# Global Exception Handling

## Why?

Centralized exception handling prevents duplicated try/catch blocks across controllers and provides consistent API error responses.

## Problem

Without centralized handling:

```csharp
try
{
    // code
}
catch(Exception ex)
{
    return StatusCode(500);
}
```

would be repeated across many controllers.

## Global Exception Handling

A middleware can catch unhandled exceptions from downstream components.

```text
Exception Middleware
        ↓
Controller
        ↓
Service
        ↓
Repository
        ↓
Exception
        ↑
Middleware catches it
```

## ProblemDetails

ProblemDetails provides a standardized format for HTTP API errors.

Example:

```json
{
  "title": "Not Found",
  "status": 404,
  "detail": "Task was not found."
}
```

## Important Status Codes

- 200 - OK
- 201 - Created
- 204 - No Content
- 400 - Bad Request
- 401 - Unauthorized
- 403 - Forbidden
- 404 - Not Found
- 409 - Conflict
- 500 - Internal Server Error

## 401 vs 403

401:

> User is not authenticated.

403:

> User is authenticated but does not have permission.

## Exception vs Validation

Validation errors are expected input problems and should normally result in 400.

Unexpected failures should be handled as exceptions and normally result in 500.

## Security

Do not expose stack traces or internal exception details to API clients in production.

Log detailed exception information internally and return a safe response to the client.