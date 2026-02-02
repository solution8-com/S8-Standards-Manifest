# C#/.NET Coding Standards

## Overview

C# and .NET are used for enterprise applications, backend services, and Windows-based solutions. We follow Microsoft's C# Coding Conventions with modern best practices.

## .NET Version

- Use **.NET 8** or newer for all new projects
- Leverage modern C# features (C# 12+)

## Code Style

### Naming Conventions
```csharp
// PascalCase for classes, interfaces, methods, and properties
public class UserService
{
    public string GetUserName(string userId)
    {
        return _repository.FetchName(userId);
    }
}

// Interface prefix with 'I'
public interface IUserRepository
{
    User GetById(string id);
}

// camelCase with underscore for private fields
private readonly IUserRepository _userRepository;
private string _userName;

// PascalCase for public properties
public string UserName { get; set; }

// PascalCase for constants
public const int MaxRetryCount = 3;
public const string ApiEndpoint = "https://api.example.com";

// Namespace matches folder structure
namespace Solution8.Users.Services;
```

### Formatting
- Use 4 spaces for indentation
- Opening brace on new line (Allman style)
- Maximum line length: 120 characters

```csharp
public class Example
{
    public void Method()
    {
        if (condition)
        {
            DoSomething();
        }
        else
        {
            DoSomethingElse();
        }
    }
}
```

## Modern C# Features

### Records
```csharp
// Use records for immutable data
public record User(string Id, string Name, string Email);

// Record with validation
public record User(string Id, string Name, string Email)
{
    public User
    {
        ArgumentNullException.ThrowIfNull(Id);
        ArgumentNullException.ThrowIfNull(Email);
    }
}

// Record class with methods
public record class UserDto(string Id, string Name, string Email)
{
    public bool IsValid() => !string.IsNullOrEmpty(Email) && Email.Contains('@');
}
```

### Pattern Matching
```csharp
// Pattern matching with switch expressions
public string DescribeObject(object obj) => obj switch
{
    string s => $"String: {s}",
    int i => $"Number: {i}",
    User { Name: var name } => $"User: {name}",
    null => "Null value",
    _ => "Unknown type"
};

// Property patterns
public decimal CalculateDiscount(Order order) => order switch
{
    { Total: > 1000, CustomerType: "Premium" } => order.Total * 0.2m,
    { Total: > 500 } => order.Total * 0.1m,
    _ => 0
};
```

### Nullable Reference Types
```csharp
#nullable enable

public class UserService
{
    // Non-nullable
    public User GetUser(string id)
    {
        var user = _repository.Find(id);
        return user ?? throw new NotFoundException($"User {id} not found");
    }
    
    // Nullable
    public User? FindUser(string id)
    {
        return _repository.Find(id);
    }
}
```

### File-Scoped Namespaces (C# 10+)
```csharp
namespace Solution8.Users.Services;

public class UserService
{
    // Class implementation
}
```

### Required Properties (C# 11+)
```csharp
public class User
{
    public required string Id { get; init; }
    public required string Name { get; init; }
    public string? Email { get; init; }
}

// Usage - compiler ensures required properties are set
var user = new User 
{ 
    Id = "123", 
    Name = "John" 
};
```

## LINQ and Collections

### LINQ Best Practices
```csharp
// Use LINQ for data queries
var activeUsers = users
    .Where(u => u.IsActive)
    .OrderBy(u => u.Name)
    .Select(u => new UserDto(u.Id, u.Name, u.Email))
    .ToList();

// Method chaining
var result = users
    .Where(u => u.Age > 18)
    .GroupBy(u => u.Role)
    .ToDictionary(g => g.Key, g => g.Count());
```

### Collection Initialization
```csharp
// Collection expressions (C# 12)
int[] numbers = [1, 2, 3, 4, 5];
List<string> names = ["Alice", "Bob", "Charlie"];

// Immutable collections
ImmutableList<string> immutableNames = ImmutableList.Create("Alice", "Bob");
```

## Async/Await

### Async Best Practices
```csharp
public async Task<User> GetUserAsync(string id, CancellationToken cancellationToken = default)
{
    try
    {
        var response = await _httpClient.GetAsync($"/users/{id}", cancellationToken);
        response.EnsureSuccessStatusCode();
        
        var user = await response.Content.ReadFromJsonAsync<User>(cancellationToken);
        return user ?? throw new InvalidOperationException("User data is null");
    }
    catch (HttpRequestException ex)
    {
        _logger.LogError(ex, "Failed to fetch user {UserId}", id);
        throw;
    }
}

// Parallel operations
public async Task<IEnumerable<User>> GetMultipleUsersAsync(IEnumerable<string> ids)
{
    var tasks = ids.Select(id => GetUserAsync(id));
    return await Task.WhenAll(tasks);
}
```

## Dependency Injection

### ASP.NET Core DI
```csharp
// Program.cs
var builder = WebApplication.CreateBuilder(args);

// Register services
builder.Services.AddScoped<IUserRepository, UserRepository>();
builder.Services.AddScoped<IUserService, UserService>();
builder.Services.AddHttpClient();

var app = builder.Build();

// Service class
public class UserService : IUserService
{
    private readonly IUserRepository _repository;
    private readonly ILogger<UserService> _logger;
    
    public UserService(IUserRepository repository, ILogger<UserService> logger)
    {
        _repository = repository;
        _logger = logger;
    }
    
    public async Task<User> CreateUserAsync(User user)
    {
        _logger.LogInformation("Creating user {UserName}", user.Name);
        return await _repository.SaveAsync(user);
    }
}
```

## Error Handling

### Custom Exceptions
```csharp
public class UserNotFoundException : Exception
{
    public string UserId { get; }
    
    public UserNotFoundException(string userId)
        : base($"User not found: {userId}")
    {
        UserId = userId;
    }
}

public class ValidationException : Exception
{
    public IEnumerable<string> Errors { get; }
    
    public ValidationException(IEnumerable<string> errors)
        : base("Validation failed")
    {
        Errors = errors;
    }
}
```

### Exception Handling
```csharp
try
{
    var user = await _userService.GetUserAsync(id);
    return Ok(user);
}
catch (UserNotFoundException ex)
{
    _logger.LogWarning(ex, "User not found: {UserId}", id);
    return NotFound(new { error = ex.Message });
}
catch (Exception ex)
{
    _logger.LogError(ex, "Unexpected error occurred");
    return StatusCode(500, new { error = "Internal server error" });
}
```

## Testing

### xUnit with Moq
```csharp
public class UserServiceTests
{
    private readonly Mock<IUserRepository> _repositoryMock;
    private readonly Mock<ILogger<UserService>> _loggerMock;
    private readonly UserService _userService;
    
    public UserServiceTests()
    {
        _repositoryMock = new Mock<IUserRepository>();
        _loggerMock = new Mock<ILogger<UserService>>();
        _userService = new UserService(_repositoryMock.Object, _loggerMock.Object);
    }
    
    [Fact]
    public async Task CreateUserAsync_WithValidData_ReturnsUser()
    {
        // Arrange
        var user = new User("1", "John", "john@example.com");
        _repositoryMock
            .Setup(r => r.SaveAsync(It.IsAny<User>()))
            .ReturnsAsync(user);
        
        // Act
        var result = await _userService.CreateUserAsync(user);
        
        // Assert
        Assert.NotNull(result);
        Assert.Equal("John", result.Name);
        _repositoryMock.Verify(r => r.SaveAsync(user), Times.Once);
    }
    
    [Theory]
    [InlineData("")]
    [InlineData(null)]
    public async Task CreateUserAsync_WithInvalidName_ThrowsException(string name)
    {
        // Arrange
        var user = new User("1", name, "john@example.com");
        
        // Act & Assert
        await Assert.ThrowsAsync<ValidationException>(() => 
            _userService.CreateUserAsync(user));
    }
}
```

## Documentation

### XML Documentation
```csharp
/// <summary>
/// Service for managing user operations.
/// </summary>
/// <remarks>
/// This service handles user creation, updates, and deletion,
/// ensuring proper validation and notification.
/// </remarks>
public class UserService : IUserService
{
    /// <summary>
    /// Creates a new user in the system.
    /// </summary>
    /// <param name="user">The user to create.</param>
    /// <param name="cancellationToken">Cancellation token.</param>
    /// <returns>The created user with generated ID.</returns>
    /// <exception cref="ValidationException">Thrown when user data is invalid.</exception>
    /// <exception cref="DuplicateUserException">Thrown when user already exists.</exception>
    public async Task<User> CreateUserAsync(User user, CancellationToken cancellationToken = default)
    {
        // Implementation
    }
}
```

## Project Structure

```
Solution8.UserService/
├── src/
│   └── Solution8.UserService/
│       ├── Controllers/
│       ├── Services/
│       ├── Repositories/
│       ├── Models/
│       ├── Dto/
│       ├── Exceptions/
│       ├── Program.cs
│       └── appsettings.json
├── tests/
│   └── Solution8.UserService.Tests/
│       ├── Services/
│       ├── Controllers/
│       └── Integration/
├── Solution8.UserService.sln
└── README.md
```

## Configuration

### appsettings.json
```json
{
  "Logging": {
    "LogLevel": {
      "Default": "Information",
      "Microsoft.AspNetCore": "Warning"
    }
  },
  "ConnectionStrings": {
    "DefaultConnection": "Server=localhost;Database=UserDb;..."
  },
  "AppSettings": {
    "MaxRetryCount": 3,
    "ApiEndpoint": "https://api.example.com"
  }
}
```

### Configuration in Code
```csharp
public class AppSettings
{
    public int MaxRetryCount { get; set; }
    public string ApiEndpoint { get; set; } = string.Empty;
}

// Program.cs
builder.Services.Configure<AppSettings>(
    builder.Configuration.GetSection("AppSettings"));

// Usage in service
public class UserService
{
    private readonly AppSettings _settings;
    
    public UserService(IOptions<AppSettings> options)
    {
        _settings = options.Value;
    }
}
```

## Code Quality Tools

- **StyleCop**: Enforce coding standards
- **FxCop/Roslyn Analyzers**: Code analysis
- **SonarQube**: Code quality and security
- **Coverlet**: Code coverage

### .editorconfig
```ini
root = true

[*.cs]
indent_style = space
indent_size = 4
end_of_line = crlf
charset = utf-8
trim_trailing_whitespace = true

# Naming conventions
dotnet_naming_rule.interface_should_be_begins_with_i.severity = warning
dotnet_naming_rule.interface_should_be_begins_with_i.symbols = interface
dotnet_naming_rule.interface_should_be_begins_with_i.style = begins_with_i

# Code style
csharp_prefer_braces = true
csharp_prefer_simple_using_statement = true
csharp_style_namespace_declarations = file_scoped
```
