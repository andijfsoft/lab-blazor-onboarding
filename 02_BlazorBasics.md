# 🧱 Phase 2: Blazor Server Basics

## ✅ Objectives

Understand how to build interactive web apps using Blazor Server, including layout, components, event handling, and data binding.

---

## 🧠 Core Concepts with Examples

### 1. ✅ Blazor Server vs Blazor WebAssembly

- **Blazor Server** runs on the server with UI interactions over SignalR.
- **Blazor WASM** runs entirely in the browser.

> 🧠 We're using **Blazor Server** for now. It offers fast load time and full .NET backend access.

---

### 2. ✅ Razor Syntax & Components

```razor
<h3>Hello, @Name!</h3>

@code {
    string Name = "Andi";
}
```

- `@code` defines the component logic.
- Razor components = `.razor` files.
- Components can be nested and reusable

### 3. ✅ Layouts and Routing

Define routes with `@page`.

```razor
@page "/dashboard"
<h3>Dashboard</h3>
```

Set up shared layout in `MainLayout.razor`:

```razor
@inherits LayoutComponentBase
<div class="container">
    @Body
</div>
```

Register routes in `App.razor`:

```razor
<Router AppAssembly="@typeof(App).Assembly">
    <Found Context="routeData">
        <RouteView RouteData="@routeData" DefaultLayout="@typeof(MainLayout)" />
    </Found>
</Router>
```

### 4. ✅ Two-way Binding

```razor
<input @bind="Email" />
<p>You typed: @Email</p>

@code {
    string Email = "";
}
```

### 5. ✅ Event Handling

```razor
<button @onclick="Increment">Count: @count</button>

@code {
    int count = 0;
    void Increment() => count++;
}
```

### 6. ✅ Dependency Injection in Blazor

```razor
@inject IUserService UserService

<p>@UserService.CurrentUserName</p>
```

Register services in `Program.cs`:

```csharp
builder.Services.AddScoped<IUserService, UserService>();
```

### 7. ✅ Lifecycle Methods

```razor
@code {
    protected override async Task OnInitializedAsync()
    {
        Users = await UserService.GetAllAsync();
    }
}
```

Use these lifecycle hooks:

- `OnInitialized` / `OnInitializedAsync`
- `OnParametersSetAsync`
- `OnAfterRenderAsync`

### 8. ✅ Forms and Validation

```razor
<EditForm Model="@model" OnValidSubmit="HandleValidSubmit">
    <InputText @bind-Value="model.Email" />
    <ValidationMessage For="@(() => model.Email)" />
    <button type="submit">Submit</button>
</EditForm>

@code {
    UserModel model = new();
    void HandleValidSubmit() => Console.WriteLine("Submitted");

    public class UserModel
    {
        [Required]
        public string Email { get; set; }
    }
}
```

### 9. ✅ Reusable Components

```razor
<!-- Button.razor -->
<button class="btn btn-primary">@Label</button>

@code {
    [Parameter] public string Label { get; set; }
}
```

Use it:

```razor
<Button Label="Save" />
```

### 10. ✅ Partial Classes for Cleaner Code

Split .razor UI and .razor.cs logic:

```csharp
public partial class Login
{
    string Email;
    void Submit() { /* login logic */ }
}
```

> `Login.razor.cs`

```razor
<h3>Login</h3>
<input @bind="Email" />
```

> `Login.razor`

---

## 🏁 Outcome

By end of this phase, you should:

- Build simple interactive Blazor pages and components
- Handle forms and validation
- Use DI and services in Blazor
- Understand the lifecycle and event handling in Blazor
