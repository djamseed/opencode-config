---
description: Apply clean code principles, idiomatic patterns, and structural consistency to C#, Go, and Zig codebases.
---

# Skill: Language-Specific Clean Code

You are an expert in writing code that is not just functional, but idiomatic and maintainable. You prioritize readability, memory safety, and structural transparency.

## Language Idioms & Standards

| Language | Philosophy          | Primary Standard                                             |
| :------- | :------------------ | :----------------------------------------------------------- |
| **C#**   | Explicit & Robust   | [Modern .NET Patterns](@knowledge/csharp-dotnet-patterns.md) |
| **Go**   | Simple & Concurrent | [Effective Go](@knowledge/go-concurrency.md)                 |
| **Zig**  | Transparent & Safe  | [Manual Resource Management](@knowledge/zig-build-system.md) |

---

## Core Principles

### 1. Meaningful Naming

| Entity         | Style       | Rule                                                             |
| :------------- | :---------- | :--------------------------------------------------------------- |
| **Interfaces** | `IFoo` (C#) | Use 'I' prefix for C#; use '-er' suffix for Go (e.g., `Reader`). |
| **Variables**  | camelCase   | Use short names in Go for small scopes; descriptive in C#.       |
| **Constants**  | PascalCase  | Avoid all-caps `SCREAMING_SNAKE` unless required by legacy.      |
| **Functions**  | Verb-based  | `CalculateTotal()`, `GetOrder()`.                                |

### 2. Method & Function Design

- **Single Responsibility**: One function = One task.
- **Limit Arguments**: If a function exceeds 3 arguments, wrap them in a `struct` (Go/Zig) or `record` (C#).
- **Early Returns**: Use "Guard Clauses" to handle errors/edge cases first and keep the "happy path" un-indented.

### 3. Error Handling Patterns

| Language | Pattern          | Implementation                                                   |
| :------- | :--------------- | :--------------------------------------------------------------- |
| **C#**   | Exceptions       | Use for truly exceptional cases; use `Result<T>` for logic flow. |
| **Go**   | Multiple Returns | `val, err := action()`. Never ignore `err`.                      |
| **Zig**  | Error Unions     | Use `try` and `catch`. Ensure errors are handled or bubbled.     |

---

## The Clean Code Workflow

### Step 1: Analyze

Check for code smells:

- **Primitive Obsession**: Using a `string` where a specific type/enum should exist.
- **Deep Nesting**: More than 3 levels of indentation is a failure.
- **Hidden Side Effects**: Functions that modify global state without warning.

### Step 2: Refactor

When refactoring, follow this sequence:

1. **Rename** for clarity.
2. **Extract** complex logic into private helper methods.
3. **Simplify** conditionals using boolean logic or polymorphism.
4. **Model** the change in **LikeC4** if the structural boundary shifted.

### Step 3: Verify

Run the language-specific quality gate:
| Language | Tool | Command |
| :--- | :--- | :--- |
| **C#** | dotnet-format | `dotnet format` |
| **Go** | gofmt / govet | `go fmt ./... && go vet ./...` |
| **Zig** | zig fmt | `zig fmt .` |

---

## Quality Standards for Agents

- **No Comments for Obvious Code**: Code should explain "How," comments should explain "Why" (only if it's non-obvious).
- **DRY (Don't Repeat Yourself)**: If logic appears three times, abstract it.
- **YAGNI (You Ain't Gonna Need It)**: Do not add abstractions for "future use cases" that don't exist yet.
