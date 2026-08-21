# Authentication & JWT

## Authentication

Authentication verifies the identity of a user.

> Who are you?

## Authorization

Authorization determines whether an authenticated user has permission to perform an operation.

> What are you allowed to do?

## JWT

JWT stands for JSON Web Token.

It is commonly used to securely transmit claims between a client and server.

Structure:

```text
HEADER.PAYLOAD.SIGNATURE
```

## Header

Contains metadata such as:

```json
{
  "alg": "HS256",
  "typ": "JWT"
}
```

## Payload

Contains claims:

```json
{
  "sub": "123",
  "role": "Admin"
}
```

## Signature

Used to verify that the token has not been modified and was signed by a trusted party.

## JWT Is Not Encryption

JWT payloads are generally encoded, not encrypted.

Do not put sensitive information such as passwords or secrets in the payload.

## JWT Request

```http
Authorization: Bearer <token>
```

## Authentication Flow

```text
Request
   ↓
Authentication Middleware
   ↓
Read JWT
   ↓
Validate token
   ↓
Create ClaimsPrincipal
   ↓
Authorization
   ↓
Controller
```

## 401 vs 403

### 401 Unauthorized

The client is not successfully authenticated.

Examples:

- Missing token
- Invalid token
- Expired token

### 403 Forbidden

The user is authenticated but does not have sufficient permissions.

## `[Authorize]`

```csharp
[Authorize]
```

requires an authenticated user.

## Role-Based Authorization

```csharp
[Authorize(Roles = "Admin")]
```

requires the authenticated user to have the Admin role.

## Middleware Order

```csharp
app.UseAuthentication();
app.UseAuthorization();
app.MapControllers();
```

Authentication must establish the user's identity before authorization evaluates permissions.