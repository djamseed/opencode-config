# C# & .NET Engineering Standards

| Focus Area           | Standard                                                           |
| :------------------- | :----------------------------------------------------------------- |
| **Language Version** | C# 12/13+ (Modern features preferred)                              |
| **Framework**        | .NET 8/9+ Core                                                     |
| **DI Pattern**       | Constructor injection via Microsoft.Extensions.DependencyInjection |
| **Async**            | Task-based (TAP) with `ValueTask` for hot paths                    |

## Structural Rules

- **File-Scoped Namespaces**: Use `namespace MyProject.Core;` to reduce indentation.
- **Primary Constructors**: Use for classes with simple dependency injection.
- **Records**: Use `record` or `record struct` for immutable data transfer objects.

## CLI-First Management

| Task            | Command                     |
| :-------------- | :-------------------------- |
| **New Project** | `dotnet new [template]`     |
| **Add Package** | `dotnet add package [name]` |
| **Testing**     | `dotnet test`               |
| **Format**      | `dotnet format`             |
