# EF Core Migration Flow

EF Core migrations synchronize the application's model with the database schema.

## Flow

```text
C# Entity
    ↓
DbContext
    ↓
EF Core Model
    ↓
Migration
    ↓
Database Update
    ↓
Database Schema
```

## Create Migration

```bash
dotnet ef migrations add InitialCreate \
  --project TaskManagement.Infrastructure \
  --startup-project TaskManagement.API
```

This generates migration files describing schema changes.

## Apply Migration

```bash
dotnet ef database update \
  --project TaskManagement.Infrastructure \
  --startup-project TaskManagement.API
```

This applies pending migrations to the database.

## Migration History

EF Core maintains:

```text
__EFMigrationsHistory
```

This table records migrations that have already been applied.

## Important

A migration can exist in the codebase without being applied to the database.

Therefore:

```text
Migration created ≠ Database updated
```

## Troubleshooting

If the application reports:

```text
relation "Tasks" does not exist
```

check:

1. Whether the migration contains `CreateTable("Tasks")`.
2. Whether the migration has been applied.
3. Whether the application and EF tools use the same database.
4. Whether the expected table exists in the expected schema.

## Production Rule

Do not remove/rewrite migrations that have already been applied to shared or production databases.

Instead, create a new migration containing the required schema changes.