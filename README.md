# OpenCode Configuration

Personal configuration for [OpenCode](https://opencode.ai), an AI-powered coding assistant.

## Overview

This repository contains my custom OpenCode setup with:

| Component | Description |
| :-------- | :---------- |
| **Agents** | Custom agents including `architect` for high-level technical design |
| **Skills** | Domain-specific instructions (backend-engineer, clean-code) |
| **Knowledge** | Reference docs for C#, Go, Zig, and LikeC4 DSL |
| **MCP Tools** | Context7, gh_grep, fetch, thinking |
| **Providers** | Anthropic, Google (Gemini), Ollama (local) |

## Primary Stack

- **C#** (modern .NET with Minimal APIs)
- **Go** (Chi/std routing)
- **Zig** (explicit memory management)
- **Ollama** (local AI with model switching)
- **LikeC4** (architecture-as-code diagramming)

## Setup

1. **Install OpenCode**:
   ```bash
   npm install -g opencode
   ```

2. **Configure OpenCode**:
   Copy this repo to `~/.config/opencode/`:
   ```bash
   cp -r .config/opencode ~/
   ```

3. **Install Dependencies**:
   ```bash
   cd ~/.config/opencode
   npm install
   ```

4. **Ollama Setup** (for local models):
   ```bash
   ollama pull gemma4:e4b
   ```

## Configuration

| File | Purpose |
| :--- | :------- |
| `opencode.json` | Main config: providers, models, MCP, permissions |
| `AGENTS.md` | Global agent instructions and workflow rules |
| `agents/*.md` | Custom agent definitions |
| `skills/*.md` | Skill-specific instructions |
| `knowledge/*.md` | Domain reference docs |

## Usage

```bash
opencode                    # Start interactive session
opencode --agent architect  # Use specific agent
opencode --file path/to/xyz # Execute on specific file
```

## Key Principles

- **Table-first reporting**: Convert structured lists to tables
- **Architect first**: Use LikeC4 for system modeling before implementation
- **Local-first**: Prefer Ollama for fast tasks, cloud models for complex reasoning
- **Explicit**: No magic; clear error handling and structured logging