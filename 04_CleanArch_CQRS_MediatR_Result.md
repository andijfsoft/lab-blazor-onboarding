# 🧱 Phase 4: Clean Architecture, CQRS, MediatR & Result Pattern

## ✅ Objectives

Learn how to structure a scalable, maintainable Blazor Server application using:

- Clean Architecture principles
- CQRS with MediatR
- Result pattern for error handling

---

## 📐 Clean Architecture

**Folder Structure:**

```md
src/
├── Application/ # Business logic, CQRS commands/queries
├── Domain/ # Entities, value objects, enums
├── Infrastructure/ # Data access, EF Core, external services
├── Web/ # Blazor Server app
└── Shared/ # Shared types between layers
```

**Layer Responsibilities:**

- **Domain**: Core logic, no dependencies
- **Application**: UseCases (Commands/Queries), interfaces
- **Infrastructure**: Implement services/interfaces (e.g., Repositories, Email, Db)
- **Web**: UI and DI setup

---

## 🧾 CQRS (Command Query Responsibility Segregation)

**Query:** Retrieves data  
**Command:** Modifies state

---

## 🤝 MediatR Setup

Install:

```bash
dotnet add package MediatR
dotnet add package MediatR.Extensions.Microsoft.DependencyInjection
```

In `Program.cs`:

```csharp
builder.Services.AddMediatR(cfg =>
    cfg.RegisterServicesFromAssembly(typeof(SomeCommand).Assembly));
```

## ✅ Command Example

### 1. CreateProductCommand.cs

```csharp
public record CreateProductCommand(string Name) : IRequest<Result<int>>;
```

### 2. CreateProductHandler.cs

```csharp
public class CreateProductHandler : IRequestHandler<CreateProductCommand, Result<int>>
{
    private readonly AppDbContext _db;

    public CreateProductHandler(AppDbContext db)
    {
        _db = db;
    }

    public async Task<Result<int>> Handle(CreateProductCommand request, CancellationToken cancellationToken)
    {
        if (string.IsNullOrWhiteSpace(request.Name))
            return Result.Failure<int>("Product name is required");

        var product = new Product { Name = request.Name };
        _db.Products.Add(product);
        await _db.SaveChangesAsync(cancellationToken);

        return Result.Success(product.Id);
    }
}
```

## 🔎 Query Example

### 1. GetProductsQuery.cs

```csharp
public record GetProductsQuery : IRequest<List<Product>>;
```

### 1. GetProductsHandler.cs

```csharp
public class GetProductsHandler : IRequestHandler<GetProductsQuery, List<Product>>
{
    private readonly AppDbContext _db;

    public GetProductsHandler(AppDbContext db)
    {
        _db = db;
    }

    public async Task<List<Product>> Handle(GetProductsQuery request, CancellationToken cancellationToken)
    {
        return await _db.Products.ToListAsync(cancellationToken);
    }
}
```

## 🎯 Using It in Blazor

```razor
@inject IMediator Mediator

@code {
    private List<Product> products = new();

    protected override async Task OnInitializedAsync()
    {
        products = await Mediator.Send(new GetProductsQuery());
    }

    private async Task CreateProduct()
    {
        var result = await Mediator.Send(new CreateProductCommand("Test Product"));
        if (result.IsFailure)
        {
            // Show result.Error
        }
    }
}
```

## 🧱 Result Pattern

Result.cs (Simplified)

```csharp
public class Result
{
    public bool IsSuccess { get; }
    public string Error { get; }

    public bool IsFailure => !IsSuccess;

    protected Result(bool isSuccess, string error)
    {
        IsSuccess = isSuccess;
        Error = error;
    }

    public static Result Success() => new(true, string.Empty);
    public static Result Failure(string error) => new(false, error);
}

public class Result<T> : Result
{
    public T Value { get; }

    protected Result(T value, bool isSuccess, string error)
        : base(isSuccess, error)
    {
        Value = value;
    }

    public static Result<T> Success(T value) => new(value, true, string.Empty);
    public static new Result<T> Failure(string error) => new(default!, false, error);
}
```

---

## 🏁 Outcome

By the end of this phase, you should be able to:

- Structure a Blazor Server app using Clean Architecture
- Implement CQRS with MediatR to separate logic
- Apply Result pattern for robust error management
