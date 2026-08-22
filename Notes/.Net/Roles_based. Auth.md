# Claims, Roles & Policy-Based Authorization

## Claim

A claim is a piece of information about an authenticated user.

Examples:

```text
User ID
Username
Role
Permission
Tenant ID
```

Access claims:

```csharp
User.Identity?.Name;

User.FindFirst(ClaimTypes.Role)?.Value;
```

## Role-Based Authorization

```csharp
[Authorize(Roles = "Admin")]
```

Allows only users with the Admin role.

Multiple roles:

```csharp
[Authorize(Roles = "Admin,Manager")]
```

means Admin OR Manager.

## Policy-Based Authorization

Policies define authorization requirements.

Example:

```csharp
builder.Services.AddAuthorization(options =>
{
    options.AddPolicy("CanManageTasks", policy =>
    {
        policy.RequireClaim(
            "permission",
            "tasks.manage");
    });
});
```

Use:

```csharp
[Authorize(Policy = "CanManageTasks")]
```

## Roles vs Policies

Roles are useful for simple role-based access.

Policies are more flexible and can express requirements based on:

- Claims
- Roles
- Custom requirements
- Custom authorization handlers

## 401 vs 403

### 401

Authentication failed.

Examples:

- Missing token
- Invalid token
- Expired token

### 403

Authentication succeeded but authorization failed.

Example:

```text
User role = User
Required role = Admin
```

## Authentication vs Authorization

```text
Authentication
    ↓
Who are you?
    ↓
ClaimsPrincipal
    ↓
Authorization
    ↓
What are you allowed to do?
```

## Important

Authorization at the API boundary can be implemented using `[Authorize]`, roles, and policies.

Core business rules should still be enforced in the appropriate Application/Domain layer rather than relying only on controller attributes.