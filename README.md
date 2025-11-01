# 🏗️ ASP.NET Clean Architecture Scaffolding Script

A comprehensive Bash script that automates the creation of a complete ASP.NET Web API solution following Clean Architecture principles.

## 📋 Table of Contents

- [Features](#features)
- [Prerequisites](#prerequisites)
- [Installation](#installation)
- [Usage](#usage)
- [Architecture Overview](#architecture-overview)
- [Examples](#examples)
- [What Gets Created](#what-gets-created)
- [Customization Options](#customization-options)
- [Troubleshooting](#troubleshooting)

## ✨ Features

- **🏛️ Clean Architecture**: Properly structured layers (Domain, Application, Infrastructure, WebApi)
- **📦 Automatic Package Installation**: Installs all necessary NuGet packages
- **🔗 Project References**: Automatically configures correct dependencies between layers
- **🗄️ Multiple Database Providers**: Supports SQL Server, PostgreSQL, and SQLite
- **🎯 CQRS Support**: Optional MediatR integration for Command Query Responsibility Segregation
- **🧪 Test Projects**: Optional xUnit test projects with essential testing packages
- **📝 Starter Files**: Creates base entities, repositories, DbContext, and DI configurations
- **🎨 Interactive CLI**: Colorful, user-friendly command-line interface
- **✅ Idempotent**: Safe to re-run with warnings for existing files
- **📚 Documentation**: Generates README with getting started instructions

## 📋 Prerequisites

Before running the script, ensure you have:

- **.NET SDK** (version 6.0 or later)

  - Check: `dotnet --version`
  - Install: https://dotnet.microsoft.com/download

- **Bash** (Linux, macOS, WSL, or Git Bash on Windows)

- **Database** (depending on your choice):
  - SQL Server or LocalDB
  - PostgreSQL
  - SQLite (no installation required)

## 🚀 Installation

1. **Download the script:**

   ```bash
   curl -O https://raw.githubusercontent.com/your-repo/scaffold-clean-api.sh
   # Or download manually and save as scaffold-clean-api.sh
   ```

2. **Make it executable:**

   ```bash
   chmod +x scaffold-clean-api.sh
   ```

3. **Verify it works:**
   ```bash
   ./scaffold-clean-api.sh --help
   ```

## 📖 Usage

### Basic Syntax

```bash
./scaffold-clean-api.sh --name <SolutionName> [OPTIONS]
```

### Command-Line Options

| Option                  | Description                                     | Required | Default     |
| ----------------------- | ----------------------------------------------- | -------- | ----------- |
| `--name <name>`         | Name of the solution                            | ✅ Yes   | -           |
| `--enable-cqrs`         | Enable CQRS with MediatR                        | ❌ No    | `false`     |
| `--framework <version>` | .NET version (net6.0, net7.0, net8.0, net9.0)   | ❌ No    | `net8.0`    |
| `--db-provider <type>`  | Database provider (sqlserver, postgres, sqlite) | ❌ No    | `sqlserver` |
| `--include-tests`       | Include test projects                           | ❌ No    | `false`     |
| `--help`                | Display help message                            | ❌ No    | -           |

## 🏛️ Architecture Overview

The script creates a solution following Clean Architecture principles:

```
┌─────────────────────────────────────────────────────┐
│                      WebApi                         │
│              (Controllers, Middleware)              │
└──────────────┬────────────────────┬─────────────────┘
               │                    │
               ▼                    ▼
┌──────────────────────┐  ┌──────────────────────────┐
│    Application       │  │    Infrastructure        │
│  (Use Cases, DTOs)   │  │  (Data, Repositories)    │
└──────┬───────────────┘  └────────┬─────────────────┘
       │                           │
       │         ┌─────────────────┘
       │         │
       ▼         ▼
┌─────────────────────────────────────┐
│             Domain                  │
│  (Entities, Interfaces, Rules)      │
└─────────────────────────────────────┘
```

### Layer Responsibilities

#### 🔵 **Domain Layer** (No dependencies)

- Core business entities
- Domain interfaces
- Value objects
- Business rules

#### 🟢 **Application Layer** (Depends on: Domain)

- Use cases/business logic
- DTOs (Data Transfer Objects)
- Service interfaces
- Mappings & validations
- Commands & queries (if CQRS enabled)

#### 🟡 **Infrastructure Layer** (Depends on: Application, Domain)

- Data access (DbContext, Repositories)
- External services
- Third-party integrations
- Configuration implementations

#### 🔴 **WebApi Layer** (Depends on: Application, Infrastructure)

- REST API controllers
- Middleware
- Request/response models
- API configuration

## 💡 Examples

### Example 1: Basic Setup

Create a simple Web API with SQL Server:

```bash
./scaffold-clean-api.sh --name MyShopApi
```

**Creates:**

- Solution with 4 projects
- SQL Server configuration
- Basic starter files

### Example 2: CQRS Enabled

Create with CQRS pattern using MediatR:

```bash
./scaffold-clean-api.sh --name MyShopApi --enable-cqrs
```

**Adds:**

- MediatR packages
- Commands/ and Queries/ folders
- Handler structure

### Example 3: PostgreSQL with Tests

Create with PostgreSQL and test projects:

```bash
./scaffold-clean-api.sh --name MyShopApi \
  --db-provider postgres \
  --include-tests
```

**Adds:**

- PostgreSQL configuration
- 3 test projects (Domain, Application, WebApi)
- Testing packages (xUnit, Moq, FluentAssertions)

### Example 4: Full Featured

Create with all features:

```bash
./scaffold-clean-api.sh --name MyShopApi \
  --enable-cqrs \
  --framework net8.0 \
  --db-provider postgres \
  --include-tests
```

**Creates a complete solution with:**

- CQRS pattern
- PostgreSQL
- .NET 8.0
- Comprehensive tests

### Example 5: SQLite for Development

Create with SQLite (no external database needed):

```bash
./scaffold-clean-api.sh --name MyShopApi \
  --db-provider sqlite \
  --enable-cqrs \
  --include-tests
```

## 📦 What Gets Created

### 📂 Project Structure

```
MyShopApi/
├── src/
│   ├── MyShopApi.Domain/
│   │   ├── Entities/
│   │   │   └── BaseEntity.cs
│   │   ├── ValueObjects/
│   │   ├── Enums/
│   │   └── Interfaces/
│   │       └── IRepository.cs
│   │
│   ├── MyShopApi.Application/
│   │   ├── Interfaces/
│   │   ├── DTOs/
│   │   ├── Services/
│   │   ├── Mappings/
│   │   ├── Validators/
│   │   ├── Commands/          # If --enable-cqrs
│   │   ├── Queries/           # If --enable-cqrs
│   │   ├── Handlers/          # If --enable-cqrs
│   │   └── DependencyInjection.cs
│   │
│   ├── MyShopApi.Infrastructure/
│   │   ├── Data/
│   │   │   └── ApplicationDbContext.cs
│   │   ├── Repositories/
│   │   │   └── Repository.cs
│   │   ├── Services/
│   │   ├── Configurations/
│   │   └── DependencyInjection.cs
│   │
│   └── MyShopApi.WebApi/
│       ├── Controllers/
│       ├── Middleware/
│       ├── Filters/
│       ├── Extensions/
│       ├── Program.cs
│       └── appsettings.json
│
├── tests/                     # If --include-tests
│   ├── MyShopApi.Domain.Tests/
│   ├── MyShopApi.Application.Tests/
│   └── MyShopApi.WebApi.Tests/
│
├── MyShopApi.sln
├── README.md
└── .gitignore
```

### 📦 Installed NuGet Packages

#### Application Layer

- `AutoMapper`
- `AutoMapper.Extensions.Microsoft.DependencyInjection`
- `FluentValidation`
- `FluentValidation.DependencyInjectionExtensions`
- `MediatR` (if --enable-cqrs)
- `MediatR.Extensions.Microsoft.DependencyInjection` (if --enable-cqrs)

#### Infrastructure Layer

- `Microsoft.EntityFrameworkCore.SqlServer` (or PostgreSQL/SQLite)
- `Microsoft.EntityFrameworkCore.Design`
- `Microsoft.EntityFrameworkCore.Tools`

#### WebApi Layer

- `Swashbuckle.AspNetCore`
- `Microsoft.AspNetCore.Authentication.JwtBearer`

#### Test Projects (if --include-tests)

- `xUnit`
- `Moq`
- `FluentAssertions`
- `Microsoft.AspNetCore.Mvc.Testing` (WebApi tests)
- `Microsoft.EntityFrameworkCore.InMemory` (WebApi tests)

### 📄 Starter Files Created

1. **BaseEntity.cs** - Base class for all entities with Id, CreatedAt, UpdatedAt
2. **IRepository<T>** - Generic repository interface
3. **Repository<T>** - Generic repository implementation
4. **ApplicationDbContext** - EF Core DbContext with auto-configuration
5. **DependencyInjection.cs** - DI setup for each layer
6. **Program.cs** - Configured with Swagger and DI
7. **appsettings.json** - With connection string template
8. **README.md** - Project documentation
9. **.gitignore** - Standard .NET gitignore

## 🔧 Customization Options

### Database Providers

#### SQL Server (default)

```bash
--db-provider sqlserver
```

Connection string: `Server=(localdb)\mssqllocaldb;Database=YourDb;Trusted_Connection=True`

#### PostgreSQL

```bash
--db-provider postgres
```

Connection string: `Host=localhost;Database=YourDb;Username=postgres;Password=postgres`

#### SQLite

```bash
--db-provider sqlite
```

Connection string: `Data Source=YourDb.db`

### .NET Framework Versions

Supported versions:

- `net6.0` - .NET 6 LTS
- `net7.0` - .NET 7
- `net8.0` - .NET 8 LTS (default)
- `net9.0` - .NET 9

Example:

```bash
./scaffold-clean-api.sh --name MyApi --framework net7.0
```

## 🚀 After Creation

### 1. Navigate to Your Project

```bash
cd MyShopApi
```

### 2. Create Initial Migration

```bash
dotnet ef migrations add InitialCreate \
  --project src/MyShopApi.Infrastructure \
  --startup-project src/MyShopApi.WebApi
```

### 3. Update Database

```bash
dotnet ef database update \
  --project src/MyShopApi.Infrastructure \
  --startup-project src/MyShopApi.WebApi
```

### 4. Run the Application

```bash
dotnet run --project src/MyShopApi.WebApi
```

### 5. Access Swagger UI

Open browser: `https://localhost:5001/swagger`

### 6. Run Tests (if included)

```bash
dotnet test
```

## 🛠️ Troubleshooting

### Issue: Permission Denied

**Problem:** `bash: ./scaffold-clean-api.sh: Permission denied`

**Solution:**

```bash
chmod +x scaffold-clean-api.sh
```

### Issue: .NET SDK Not Found

**Problem:** `Error: .NET SDK is not installed`

**Solution:** Install .NET SDK from https://dotnet.microsoft.com/download

### Issue: Directory Already Exists

**Problem:** Warning about existing directory

**Solution:** The script will prompt you to continue or cancel. Type `y` to proceed or `n` to abort.

### Issue: Build Warnings

**Problem:** ⚠ Build completed with warnings

**Solution:** This is usually safe. Check the output for any critical errors. Common warnings:

- Missing XML documentation
- Unused variables in starter files

### Issue: Connection String Error

**Problem:** Cannot connect to database

**Solution:** Update connection string in `src/YourProject.WebApi/appsettings.json`:

- Verify server address
- Check credentials
- Ensure database server is running

## 🎯 Best Practices

### 1. **Follow Dependency Rules**

- Domain has NO dependencies
- Application depends only on Domain
- Infrastructure depends on Application and Domain
- WebApi depends on Application and Infrastructure

### 2. **Use Interfaces**

Define interfaces in Domain/Application, implement in Infrastructure:

```csharp
// Domain
public interface IEmailService { }

// Infrastructure
public class EmailService : IEmailService { }
```

### 3. **Keep Controllers Thin**

Controllers should delegate to services:

```csharp
[ApiController]
public class ProductsController : ControllerBase
{
    private readonly IProductService _service;

    [HttpGet]
    public async Task<IActionResult> GetAll()
        => Ok(await _service.GetAllAsync());
}
```

### 4. **Use DTOs**

Never expose entities directly:

```csharp
// Application/DTOs
public class ProductDto { }

// Use AutoMapper to map Entity <-> DTO
```

### 5. **Validate Input**

Use FluentValidation:

```csharp
public class CreateProductValidator : AbstractValidator<CreateProductDto>
{
    public CreateProductValidator()
    {
        RuleFor(x => x.Name).NotEmpty();
    }
}
```

## 📚 Additional Resources

- [Clean Architecture by Robert C. Martin](https://blog.cleancoder.com/uncle-bob/2012/08/13/the-clean-architecture.html)
- [.NET Documentation](https://learn.microsoft.com/en-us/dotnet/)
- [Entity Framework Core](https://learn.microsoft.com/en-us/ef/core/)
- [MediatR Documentation](https://github.com/jbogard/MediatR)
- [AutoMapper Documentation](https://automapper.org/)
- [FluentValidation Documentation](https://docs.fluentvalidation.net/)

## 🤝 Contributing

Suggestions and improvements are welcome! Consider:

- Additional database providers
- More starter templates
- Additional architectural patterns
- Docker support
- CI/CD configuration

## 📄 License

This script is provided as-is under the MIT License. Feel free to modify and distribute.

## 🙏 Acknowledgments

Inspired by Clean Architecture principles and the .NET community's best practices.

---

**Created with ❤️ for the .NET community**

Happy coding! 🚀
