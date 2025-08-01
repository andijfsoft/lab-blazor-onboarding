# 🗃️ Phase 3: EF Core & Identity Framework Basics

## ✅ Objectives

Learn to use Entity Framework Core for data access and ASP.NET Core Identity for authentication and user management in a Blazor Server app.

---

## 🧠 Entity Framework Core (EF Core)

### 1. ✅ Setup & Configuration

Install packages:

```bash
dotnet add package Microsoft.EntityFrameworkCore.SqlServer
dotnet add package Microsoft.EntityFrameworkCore.Tools
```

In Program.cs:

```csharp
builder.Services.AddDbContext<AppDbContext>(options =>
    options.UseSqlServer(builder.Configuration.GetConnectionString("DefaultConnection")));
```

### 2. ✅ Create a DbContext and Entity

```csharp
public class AppDbContext : DbContext
{
    public AppDbContext(DbContextOptions options) : base(options) { }

    public DbSet<Product> Products => Set<Product>();
}

public class Product
{
    public int Id { get; set; }
    public string Name { get; set; } = default!;
}
```

### 3. ✅ Migrations and Database Update

```bash
dotnet ef migrations add Initial
dotnet ef database update
```

### 4. ✅ Dependency Injection & Data Access

```csharp
@inject AppDbContext Db

<ul>
@foreach (var product in Db.Products)
{
    <li>@product.Name</li>
}
</ul>
```

## 🧠 ASP.NET Core Identity

## 1. ✅ Identity Setup

Install package:

```bash
dotnet add package Microsoft.AspNetCore.Identity.EntityFrameworkCore
```

Update AppDbContext:

```csharp
public class AppDbContext : IdentityDbContext<ApplicationUser>
{
    public AppDbContext(DbContextOptions options) : base(options) { }
}
```

Create a custom user model:

```csharp
public class ApplicationUser : IdentityUser
{
    public string FullName { get; set; } = default!;
}
```

### 2. ✅ Identity Services Configuration

In `Program.cs`:

```csharp
builder.Services.AddDefaultIdentity<ApplicationUser>(options =>
{
    options.SignIn.RequireConfirmedAccount = false;
})
.AddEntityFrameworkStores<AppDbContext>();
```

### 3. ✅ Authentication UI in Blazor

Use AuthorizeView and [Authorize]:

```razor
<AuthorizeView>
    <Authorized>
        Hello, @context.User.Identity.Name!
    </Authorized>
    <NotAuthorized>
        <a href="Identity/Account/Login">Login</a>
    </NotAuthorized>
</AuthorizeView>
```

Restrict page:

```razor
@attribute [Authorize]
@page "/dashboard"
```

### 4. ✅ Accessing Current User

```razor
@inject AuthenticationStateProvider AuthStateProvider

@code {
    private string? UserId;

    protected override async Task OnInitializedAsync()
    {
        var authState = await AuthStateProvider.GetAuthenticationStateAsync();
        var user = authState.User;

        if (user.Identity?.IsAuthenticated ?? false)
        {
            UserId = user.FindFirst(ClaimTypes.NameIdentifier)?.Value;
        }
    }
}
```

---

## 🏁 Outcome

By end of this phase, you should:

- Be able to define models and use EF Core to persist them
- Use migrations and the DbContext properly
- Understand and apply ASP.NET Core Identity for authentication
- Create secure, user-aware Blazor Server pages
