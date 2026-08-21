# JWT Authentication Implementation

## Authentication Flow

```text
Login
 ↓
Validate credentials
 ↓
Generate JWT
 ↓
Client receives token
 ↓
Client sends Bearer token
 ↓
JWT Bearer Authentication
 ↓
ClaimsPrincipal
 ↓
Authorization
 ↓
Controller
```

## Authorization Header

```http
Authorization: Bearer <JWT>
```

## `[Authorize]`

```csharp
[Authorize]
```

requires the request to be authenticated.

## Claims

Claims represent information about the authenticated user.

Example:

```text
User ID
Username
Role
```

Access claims:

```csharp
User.Identity?.Name

User.FindFirst(ClaimTypes.Role)?.Value
```

## JWT Validation

The JWT bearer handler can validate:

- Signature
- Issuer
- Audience
- Lifetime/expiration

Example:

```csharp
options.TokenValidationParameters = new TokenValidationParameters
{
    ValidateIssuer = true,
    ValidateAudience = true,
    ValidateLifetime = true,
    ValidateIssuerSigningKey = true
};
```

## JWT Is Signed, Not Necessarily Encrypted

The signature protects token integrity and authenticity.

The payload should not contain sensitive secrets because JWT payloads are generally readable after decoding.

## Security Boundary

Login endpoint:

```text
/api/auth/login
```

is public.

Protected endpoints:

```text
/api/tasks
```

use:

```csharp
[Authorize]
```

# JWT Validation

## Validation Flow

```text
JWT
 ↓
Extract token
 ↓
Validate signature
 ↓
Validate issuer
 ↓
Validate audience
 ↓
Validate lifetime
 ↓
Create ClaimsPrincipal
 ↓
HttpContext.User
```

## Issuer

Issuer identifies who issued the token.

```csharp
ValidateIssuer = true;
ValidIssuer = "...";
```

## Audience

Audience identifies the intended recipient of the token.

```csharp
ValidateAudience = true;
ValidAudience = "...";
```

## Lifetime

Checks whether the token is expired.

```csharp
ValidateLifetime = true;
```

## Signing Key

Validates that the token was signed by a trusted key.

```csharp
ValidateIssuerSigningKey = true;
```

## Symmetric Signing

Example:

```text
HS256
```

Same secret is used for signing and validation.

## Asymmetric Signing

Examples:

```text
RS256
ES256
```

Private key signs the token.

Public key validates the token.

## JWKS

JWKS stands for JSON Web Key Set.

It exposes public keys that can be used to validate tokens issued by an identity provider.

JWT header commonly contains:

```json
{
  "alg": "RS256",
  "kid": "abc123"
}
```

`kid` identifies the signing key.

## Key Rotation

Identity providers can rotate signing keys.

JWKS can expose multiple public keys, allowing APIs to validate tokens signed with different currently-valid keys.

## Important

JWT payloads should not be trusted until the token's signature and validation parameters have successfully been verified.