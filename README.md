# Identity Auth API

An ASP.NET Core 8 Web API that provides user registration, login and role-based access, plus endpoints for storing weather data and favourite cities. It is the backend for [auth-web-client](https://github.com/jvishwa06/auth-web-client).

## Features

- **Authentication**: ASP.NET Core Identity API endpoints (`MapIdentityApi`) with Entity Framework Core on PostgreSQL (Npgsql). The initial migration is in `webapi/Migrations`.
- **Roles**: admin-only endpoints for creating roles and assigning roles to users.
- **Weather data**: stores weather lookups and favourite cities in MongoDB (database `WeatherApplicationDB`, collections `WeatherApplicationData` and `FavoriteCities`).
- **Swagger UI** at `/swagger`, with a bearer-token security definition.
- **CORS** is open to any origin, method and header.

## Endpoints

| Route | Access | Purpose |
|---|---|---|
| Identity API routes (register, login and so on) | Public / authenticated | Provided by `MapIdentityApi<IdentityUser>()` |
| `GET api/User` | Authenticated | Current user |
| `POST api/User/logout` | Authenticated | Log out |
| `GET api/Role/GetRoles` | `admin` | List roles |
| `GET api/Role/GetUserRole` | Authenticated | Get a user's role |
| `POST api/Role/addRoles` | `admin` | Create roles |
| `POST api/Role/addUserRoles` | `admin` | Assign roles to users |
| `POST api/Weather/add` | None in code | Save weather data to MongoDB |
| `POST api/Weather/favorites` | None in code | Add a favourite city |
| `GET api/Weather/favorites` | None in code | List favourite cities |

## Project layout

```
webapi/
  Controllers/   User, Role and Weather controllers
  Data/          IdentityDbContext
  Migrations/    EF Core migrations
  Model/         Request models
  Services/      RoleService, MongoWeatherService
  Program.cs     Service and middleware setup
Dockerfile       Multi-stage .NET 8 build, exposes port 8080
```

## Configuration

Set these before running (in `webapi/appsettings.json`, user secrets or environment variables such as `ConnectionStrings__idenitycs`):

- `ConnectionStrings:idenitycs`: the PostgreSQL connection string (the key is spelled this way in the code).
- `ConnectionStrings:MongoDb`, or the `MONGODB_URI` environment variable: the MongoDB connection string used to store weather data and favourite cities.

`appsettings.json` contains placeholders only. Do not commit real credentials.

## Run

```bash
dotnet restore
dotnet run --project webapi
```

With Docker:

```bash
docker build -t identity-auth-api .
docker run -p 8080:8080 identity-auth-api
```
