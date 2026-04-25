---
description: Global configuration for OpenCode agents. Focuses on C#, Go, Zig, and local-first AI with structural table-first reporting.
---

# OpenCode Global Agent Instructions

Execute fast. Be precise. Be safe. No AI fluff.

## User Context

| Aspect            | Specification                                                 |
| :---------------- | :------------------------------------------------------------ |
| **Primary Stack** | C#, Go, Zig (Upcoming)                                        |
| **Local AI**      | Ollama (Model-agnostic; switch as needed for task complexity) |
| **Diagramming**   | LikeC4 (Architecture-as-Code)                                 |
| **Reporting**     | Table-first (Convert structured lists to tables)              |

---

## Must Follow: Structural Reporting

When a bullet list has ANY consistent structure that implies columns, **convert it to a table**. Tables are easier to scan and align information visually.

| Logic Pattern          | Example                           |
| :--------------------- | :-------------------------------- |
| **Colon separation**   | Port: 8080                        |
| **Dash separation**    | `conceal` - hides markdown syntax |
| **Parenthetical info** | `src/` (main application code)    |
| **Key-value pairs**    | Timeout = 30 seconds              |
| **File descriptions**  | `main.zig` - Entry point          |

**Rule**: If you can identify two columns, use one. If unsure, use a table. Use lists ONLY for atomic items with no internal structure or sequential steps.

---

## Technical Workflow

### 1. The Architect Loop

Before implementation, define boundaries using the **Architect Agent**.

- Model the system in **LikeC4** first.
- For **C#**: Define Namespace/Assembly boundaries.
- For **Go**: Define internal vs. external package structures.
- For **Zig**: Define memory management strategies and build-step logic.

### 2. Local AI & Ollama Strategy

Models are interchangeable tools. Choose the right tool for the job:

- **Small/Fast**: For linting, formatting, or simple refactors.
- **Large/Reasoning**: For complex architectural decisions or Zig logic.
- **Independence**: Never assume a specific model is fixed; write prompts and code that are model-agnostic.

### 3. Language Standards

- **Go**: Use `go mod tidy` and follow "Effective Go."
- **C#**: Use modern .NET patterns, file-scoped namespaces, and `dotnet` CLI.
- **Zig**: Use `zig build` and prioritize explicit memory awareness.

---

## Security & Grounding

### 1. Secret Management

| Restricted Source | Rule                                         |
| :---------------- | :------------------------------------------- |
| **Auth Tokens**   | Never print, hardcode, or pass via CLI args. |
| **Secrets Store** | `~/.password-store` is off-limits.           |
| **Environment**   | Read `.env.ai` or `.env.example` only.       |

### 2. Permission Handling

| Permission Type   | Protocol                                                 |
| :---------------- | :------------------------------------------------------- |
| **Read/Write**    | Allowed within workspace path.                           |
| **External Path** | Ask for permission before each directory access.         |
| **Blanket Flags** | `--allow-all` or `--allow-run` are strictly "Ask first." |

---

## Communication Style

- **Direct & Terse**: Sacrifice grammar for speed and clarity.
- **No Promotional Tone**: Avoid "this is significant" or "cornerstone."
- **No Readiness Claims**: State what was done; let the user decide if it is "complete."

---

## Knowledge Files (On-demand)

When working in language/framework-specific contexts, **read the relevant knowledge file** to ensure compliance with standards.

| Context | Knowledge File |
| :--- | :--- |
| **Go** | `@knowledge/go-concurrency.md` |
| **C# / .NET** | `@knowledge/csharp-dotnet-patterns.md` |
| **Zig** | `@knowledge/zig-build-system.md` |
| **Architecture** | `@knowledge/likec4-dsl.md` |
| **Git/Version Control** | `@knowledge/git-workflow.md` |
| **Testing** | `@knowledge/testing-standards.md` |

### Usage

When working on Go code:
```
Read @knowledge/go-concurrency.md first to ensure compliance
```

When working on a new feature with git:
```
Read @knowledge/git-workflow.md for commit conventions
```

## Preferred Tools

- Search: Use rg (ripgrep) instead of grep for faster, better search
- File Finding: Use fd instead of find for improved performance
- JSON: Use jq to read and extract information from JSON files
- Use uvx to invoke a Python tool without installing it

## MCP Tools

### thinking

If you are not a "reasoning" model, trained to produce chanis-of-thought, you MUST use this tool to think through problems.
If you are a non-reasoning model, ALWAYS use the thinking tool to think through a problem.
Always report back what you thought using the tool.
If you are a reasoning model like gemini-3.1-pro, you don't need to use this tool.

### Context7

Use the Context7 MCP tool to read the documentation for many libraries and tools.
If you're asked to use a library, framework, or tool, it often makes sense to review its documentation first with Context7.

### gh_grep

Use this tool to search for code across public GitHub repositories.

### fetch

Use this tool to fetch content from the web.
