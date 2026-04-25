# Git & Version Control Standards

| Focus Area | Standard |
| :--- | :--- |
| **Commit Format** | Conventional Commits |
| **Branch Strategy** | Feature/fix prefixes with ticket IDs |
| **Merge Strategy** | Squash & merge (linear history) |
| **Worktrees** | Use for parallel work |

---

## Conventional Commits

All commits MUST follow the [Conventional Commits](https://www.conventionalcommits.org/) specification.

### Format

```
<type>[optional scope]: <description>

[optional body]

[optional footer(s)]
```

### Types

| Type | Description |
| :--- | :--- |
| `feat` | New feature for the user |
| `fix` | Bug fix for the user |
| `docs` | Documentation only changes |
| `style` | Formatting, missing semicolons, etc. (no code change) |
| `refactor` | Code change that neither fixes a bug nor adds a feature |
| `perf` | Performance improvement |
| `test` | Adding or correcting tests |
| `build` | Changes to build system or external dependencies |
| `ci` | Changes to CI configuration files and scripts |
| `chore` | Other changes that don't modify src or test files |
| `revert` | Reverts a previous commit |

### Breaking Changes

Indicate with `!` after type/scope or `BREAKING CHANGE:` in footer:

```
feat(api)!: change response format for user endpoint

BREAKING CHANGE: The user endpoint now returns an object instead of an array.
```

### Examples

```bash
# Feature
feat(users): add email verification flow

# Bug fix with issue reference
fix(cart): prevent negative quantities

Fixes #123

# Breaking change
feat(api)!: rename endpoints to follow REST conventions

BREAKING CHANGE: All endpoints now use plural nouns.
- /user -> /users
- /product -> /products
```

---

## Branch Naming

### Format

```
<type>/<ticket-id>-<short-description>
```

### Prefixes

| Prefix | Purpose |
| :--- | :--- |
| `feature/` | New features |
| `fix/` | Bug fixes |
| `hotfix/` | Urgent production fixes |
| `refactor/` | Code refactoring |
| `docs/` | Documentation updates |
| `test/` | Test additions/updates |
| `chore/` | Maintenance tasks |

### Rules

- Use lowercase
- Use hyphens (not underscores)
- Keep descriptions short but meaningful
- Include ticket ID when available

### Examples

```
feature/PROJ-123-user-authentication
fix/PROJ-456-cart-calculation-error
hotfix/PROJ-789-payment-gateway-timeout
```

---

## Linear History (Squash & Merge)

### Strategy

- **Default**: Squash merge for feature branches
- **Result**: One commit per feature/fix in main branch
- **Benefit**: Clean, readable history

### Workflow

```bash
# Create feature branch
git checkout -b feature/PROJ-123-new-feature

# Make commits (can be messy during development)
git commit -m "wip: initial implementation"
git commit -m "wip: add tests"
git commit -m "fix: address review feedback"

# Push and create PR
git push -u origin feature/PROJ-123-new-feature

# PR gets squash-merged with clean message
```

---

## Git Configuration

```bash
# Rebase by default when pulling
git config --global pull.rebase true

# Default to main branch
git config --global init.defaultBranch main

# Auto-leave staging area clean
git config --global add.interactive.useStore true
```