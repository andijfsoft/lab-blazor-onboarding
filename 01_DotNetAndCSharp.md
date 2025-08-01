# 🧱 Phase 1: .NET & C# Fundamentals

## ✅ Objectives

Gain a strong foundation in C# and .NET features that are essential for building web apps using Blazor, EF Core, and Identity.

---

## 🧠 Core Topics and Examples

### 1. ✅ Value Types vs Reference Types

- Value types are stored in the stack, reference types in the heap.
- Modifying a value type copy does **not** affect the original.

```csharp
int x = 5;
int y = x;
y++;
Console.WriteLine(x); // 5

class Person { public string Name = "Andi"; }

Person a = new Person();
Person b = a;
b.Name = "Razif";

Console.WriteLine(a.Name); // Razif
```

### 2. ✅ Async and Await

Understand how to write non-blocking code using `Task` and `async/await`.

```csharp
public async Task LoadUsersAsync()
{
    var users = await _dbContext.Users.ToListAsync();
    Console.WriteLine($"Found {users.Count} users.");
}
```

### 3. ✅ Dependency Injection

Understand how services are registered and injected.

```csharp
// Register
builder.Services.AddScoped<IUserService, UserService>();

// Inject into Blazor
@inject IUserService UserService
```

```csharp
// Or into constructors (e.g. handlers)
public class CreateUserHandler : IRequestHandler<CreateUserCommand, Result<Guid>>
{
    private readonly IUserService _userService;

    public CreateUserHandler(IUserService userService)
    {
        _userService = userService;
    }
}
```

### 4. ✅ Classes vs Records

Records are immutable reference types and ideal for data transfer (like CQRS command/queries).

```csharp
public record CreateUserCommand(string Email, string Password);
```

```csharp
public class User
{
    public string Email { get; set; }
    public string Password { get; set; }
}
```

### 5. ✅ LINQ Basics

Use LINQ to query collections or EF Core data.

```csharp
var emails = users
    .Where(u => u.IsActive)
    .Select(u => u.Email)
    .ToList();
```

Common LINQ methods:

- `.Where()`
- `.Select()`
- `.Any()`
- `.FirstOrDefault()`
- `.OrderBy()` / `.ThenByDescending()`

### 6. ✅ Working with Collections

Get comfortable with `List<T>`, `Dictionary<TKey,TValue>`, and iteration.

```csharp
var users = new List<string> { "Ali", "Siti" };
foreach (var user in users)
{
    Console.WriteLine(user);
}
```

```csharp
var map = new Dictionary<string, int>
{
    ["admin"] = 1,
    ["staff"] = 2
};
```

### 7. ✅ Exception Handling

```csharp
try
{
    var user = await _userService.CreateAsync("test");
}
catch (UserAlreadyExistsException ex)
{
    Console.WriteLine(ex.Message);
}
```

> 🔸 Always catch specific exceptions, not just Exception.

### 8. ✅ Working with Guid, DateTime, and Enums

```csharp
Guid id = Guid.NewGuid();
DateTime now = DateTime.UtcNow;

enum Role { Admin, Staff, Member }
Role userRole = Role.Staff;
```

### 9. ✅ Null Safety and `??`, `?.`, `??=`

```csharp
string? name = null;
string display = name ?? "Unknown";

var length = name?.Length ?? 0; // Safe access
```

### 10. ✅ Project Layout

Understand project types:

- `Class Library` for reusable logic (`Core`, `Application`)
- Blazor Server for frontend UI
- Web API for APIs (if any)
- Console for scripts or seeders

---

## 🏁 Outcome

By end of this phase, you should:

- Understand C# syntax and key .NET types
- Use async/await and LINQ confidently
- Understand basic dependency injection
- Know the difference between records, classes, and how to organize code
