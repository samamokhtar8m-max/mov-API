# mov API

An ASP.NET Core Web API for a cinema booking system: manages movies, halls, customers, and tickets.

> **Type:** ASP.NET Core Web API
> **Language:** C# (.NET 8)
> **Database:** SQL Server (EF Core)
> **Docs:** Swagger / OpenAPI (dev environment)

---

## Entities

| Entity | Fields | Relationships |
|---|---|---|
| **Movie** | Title, Duration | Belongs to one `Hall`; has many `Customers` |
| **Hall** | Name, Capacity | Has many `Movies` |
| **Customer** | Name, Email | Has many `Movies`; linked to one `Ticket` |
| **Ticket** | SeatNumber, Price, ShowTime | — |

## Endpoints

| Controller | Route | Notes |
|---|---|---|
| `MovieController` | `api/Movie` | `GET`, `GET {Id}`, `POST` |
| `HallController` | `api/Hall` | `GET`, `POST` |
| `CustomerController` | `api/Customer` | `GET`, `POST` |

## Architecture

- **Generic Repository** (`IgenericReppository<T>` / `GenericReppository<T>`) — shared CRUD logic reused across entities.
- **Specification pattern** (`Specifications/`) — encapsulates query logic (filtering, includes) per entity, evaluated via `SpecifactionEvlutor`.
- **AutoMapper** (`MappingProfile`) — maps entities to DTOs (`Dto/`) for API input/output.
- **DbContext**: `StoreDbContext` (EF Core, SQL Server), with migrations already generated under `Migrations/`.

## Getting started

### Prerequisites
- .NET 8 SDK
- SQL Server (local or remote)

### Setup

1. Set your connection string in `appsettings.json` / `appsettings.Development.json` under the key the code reads via `builder.Configuration.GetConnectionString("X")` — note the connection string is currently named `"X"` in `Program.cs`; either rename it to something clearer or match it in your config.
2. Apply migrations:
   ```bash
   dotnet ef database update
   ```
3. Run the API:
   ```bash
   dotnet run
   ```
4. Browse Swagger UI (dev mode) to explore/test endpoints.

## Known limitations

- No authentication/authorization is wired up — all endpoints are open.
- The connection string key `"X"` in `Program.cs` is a placeholder name worth renaming.
- No input validation feedback beyond data annotations on the models (e.g. `[Required]`, `[MaxLength]`).
