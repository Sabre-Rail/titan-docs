# C# Coding Standard – Sabre Rail Services Ltd

**Revision:** 3
**Prepared by:** D. L. Pratt
**Date:** \[Insert Current Date]
**Company:** Sabre Rail Services Ltd

---

## Table of Contents

1. Introduction
2. Layout Conventions
3. Naming Conventions
4. Commenting Conventions
5. Language Guidelines
6. Program.cs and Startup Conventions
7. Swagger and API Documentation
8. Database Initialization and Migrations
9. Development-Time HTTP Clients
10. EditorConfig Usage

---

## 1. Introduction

### Purpose

This document defines the C# coding conventions used at Sabre Rail Services Ltd. These standards are derived from Microsoft's recommended practices and adapted for internal use.

### Goals

* Ensure consistent code formatting.
* Improve readability and maintainability.
* Facilitate collaboration across development teams.
* Support best practices in modern C# development.

---

## 2. Layout Conventions

* Use four-space indentation (tabs saved as spaces).
* One statement and one declaration per line.
* Break lines longer than 90 characters.
* Use parentheses in expressions for clarity.
* Add blank lines between method and property definitions.
* Organize code blocks with vertical spacing for readability.

```csharp
if ((value1 > value2) && (value1 > value3))
{
    // Take appropriate action.
}
```

---

## 3. Naming Conventions

### Capitalization Styles

* **PascalCase**:

  * Class, record, struct, interface names
  * Method and property names
  * Enum types and members
* **camelCase**:

  * Parameters and local variables
  * Private fields (prefixed with `_`)
* **Prefix Conventions**:

  * Static fields: `s_`
  * Thread-static fields: `t_`

### Acronyms

* Two-letter acronyms: all uppercase (e.g., `IOStream`)
* Longer acronyms: capitalize only first letter (e.g., `XmlReader`)

### Enums

* Use singular names unless marked with `[Flags]`
* Use PascalCase for members
* Do not prefix enum values with the enum type

```csharp
public enum Color
{
    Red,
    Green,
    Blue
}

[Flags]
public enum FileAccess
{
    Read = 1,
    Write = 2,
    Execute = 4
}
```

---

## 4. Commenting Conventions

* Use full sentences with proper punctuation.
* Place comments above the relevant code.
* Avoid decorative comment blocks.
* Use XML documentation for public members.

```csharp
// Calculates the total cost including tax.
public decimal CalculateTotal(decimal amount, decimal taxRate)
```

---

## 5. Language Guidelines

### Strings

* Use string interpolation:

```csharp
string message = $"Hello, {name}";
```

* Use `StringBuilder` for concatenation in loops.

### var Usage

* Use `var` when the type is obvious:

```csharp
var count = 42;
```

* Use explicit types when clarity is needed:

```csharp
int total = CalculateTotal();
```

### Arrays

```csharp
string[] vowels = { "a", "e", "i", "o", "u" };
```

### Delegates

* Use `Func<>` and `Action<>` instead of custom delegate types.

### Whitespace

* Use whitespace to separate logical blocks and improve readability.
* Align similar lines of code vertically where appropriate.

---

## 6. Program.cs and Startup Conventions

Structure the `Program.cs` file using logical sections with comments:

```csharp
// 1. Configuration
// 2. Authentication
// 3. Authorization
// 4. HttpClient
// 5. Event Store
// 6. Services
// 7. Swagger
// 8. CORS
// 9. Migrations
```

* Consider extracting setup into extension methods for clarity.

---

## 7. Swagger and API Documentation

* Use `AddSwaggerGen()` to register Swagger.
* Define JWT bearer token scheme:

```csharp
c.AddSecurityDefinition("Bearer", new OpenApiSecurityScheme {
    Name = "Authorization",
    Type = SecuritySchemeType.Http,
    Scheme = "Bearer",
    In = ParameterLocation.Header,
    Description = "Enter JWT as: Bearer {your_token}"
});
```

* Read server URL from configuration for Docker support.

---

## 8. Database Initialization and Migrations

* Use `CreateScope()` to get scoped services.
* Catch and log exceptions during migration.
* Use raw SQL with care for custom migrations.

```csharp
await store.Storage.ApplyAllConfiguredChangesToDatabaseAsync();
```

---

## 9. Development-Time HTTP Clients

* For SSL bypass in development:

```csharp
builder.Services.AddHttpClient("NoSSL")
    .ConfigurePrimaryHttpMessageHandler(() =>
        new HttpClientHandler {
            ServerCertificateCustomValidationCallback = (_, _, _, _) => true
        });
```

* Clearly label this as **not for production**.
* Wrap registration in development environment checks.

---

## 10. EditorConfig Usage

* Use `.editorconfig` to enforce style rules project-wide.
* Specify naming conventions, indentation, newline preferences, etc.
* Ensure IDEs and editors are configured to respect `.editorconfig` settings.

Example:

```ini
[*.cs]
indent_style = space
indent_size = 4
csharp_new_line_before_open_brace = all
csharp_style_var_for_built_in_types = true:suggestion
csharp_style_var_elsewhere = false:suggestion
```

---

**End of Document**
