# Go Standards

| Focus Area | Standard |
| :--- | :--- |
| **Runtime** | Go 1.21+ |
| **Linting** | golangci-lint v2+ |
| **Testing** | testify |
| **Project Structure** | `cmd/`, `internal/`, `pkg/` |

---

## Runtime & Tooling

- **Go version**: 1.21+ (use Go 1.25+ where possible)
- **Module mode**: Always use Go modules (`go.mod`)
- **Linting**: golangci-lint v2+

### Common Commands

```bash
go mod init <module-path>
go mod tidy
go mod download

go build ./...
go run .
go test -race ./... -coverprofile=coverage.out
go test -bench=. -benchmem ./...
```

---

## Code Quality: golangci-lint (v2+)

### Install

```bash
go install github.com/golangci/golangci-lint/v2/cmd/golangci-lint@latest
```

### Configuration (`.golangci.yml`)

```yaml
version: "2"

run:
  timeout: 5m
  go: "1.21"

linters:
  enable:
    - errcheck
    - gosimple
    - govet
    - ineffassign
    - staticcheck
    - unused
    - bodyclose
    - contextcheck
    - errorlint
    - gocritic
    - gofmt
    - goimports
    - misspell
    - revive
    - rowserrcheck

linters-settings:
  goimports:
    local-prefixes: github.com/yourorg
  gocritic:
    enabled-tags:
      - diagnostic
      - style
      - performance
  revive:
    rules:
      - name: context-as-argument
      - name: error-return
      - name: exported
      - name: var-naming

issues:
  exclude-rules:
    - path: _test\.go
      linters:
        - gosec
```

### Run

```bash
golangci-lint run ./...
golangci-lint run --fix ./...
```

---

## Testing with testify

### Test File Structure

```go
package user_test

import (
    "context"
    "testing"

    "github.com/stretchr/testify/assert"
    "github.com/stretchr/testify/require"
    "github.com/stretchr/testify/mock"

    "github.com/yourorg/project/internal/user"
)

func TestCreateUser(t *testing.T) {
    t.Parallel()

    tests := []struct {
        name    string
        input  user.CreateInput
        want   *user.User
        wantErr error
    }{
        {
            name:  "valid user",
            input: user.CreateInput{Name: "Alice", Email: "alice@example.com"},
            want:  &user.User{Name: "Alice", Email: "alice@example.com"},
        },
        {
            name:    "empty name",
            input:   user.CreateInput{Name: "", Email: "alice@example.com"},
            wantErr: user.ErrInvalidName,
        },
    }

    for _, tt := range tests {
        t.Run(tt.name, func(t *testing.T) {
            t.Parallel()

            got, err := user.Create(context.Background(), tt.input)

            if tt.wantErr != nil {
                require.ErrorIs(t, err, tt.wantErr)
                return
            }

            require.NoError(t, err)
            assert.Equal(t, tt.want.Name, got.Name)
            assert.Equal(t, tt.want.Email, got.Email)
        })
    }
}
```

### Mock Pattern

```go
type MockRepository struct {
    mock.Mock
}

func (m *MockRepository) GetUser(ctx context.Context, id string) (*User, error) {
    args := m.Called(ctx, id)
    if args.Get(0) == nil {
        return nil, args.Error(1)
    }
    return args.Get(0).(*User), args.Error(1)
}
```

### Assert vs Require

| Function | Use When |
| :--- | :--- |
| `assert.*` | Check continued assertions on failure |
| `require.*` | Stop test immediately on failure |

---

## Concurrency Patterns

| Concept | Best Practice |
| :--- | :--- |
| **Goroutines** | Never start a goroutine without knowing how it will stop. |
| **Channels** | Use for orchestration; use Mutexes for state protection. |
| **Context** | Pass `context.Context` as the first argument to all IO/long-running functions. |
| **Error Handling** | Wrap errors with `fmt.Errorf("context: %w", err)`. |

### Safety Patterns

- **Select/Done**: Use `done` or `ctx.Done()` channel in `select` loops.
- **WaitGroups**: Use `sync.WaitGroup` for parallel worker synchronization.
- **Atomic**: Use `sync/atomic` for simple counters.

---

## Project Structure

```
cmd/           # One subdirectory per binary
internal/      # Code not exported to other projects
pkg/           # Code safe for external consumption
```

---

## Idiomatic Go Patterns

| Pattern | Guidance |
| :--- | :--- |
| **Interfaces** | Interface composition over inheritance |
| **Accept interfaces** | Accept interfaces, return structs |
| **Errors** | Error values over exceptions |
| **Explicit** | Explicit over implicit behavior |
| **Small interfaces** | Prefer small, focused interfaces |

---

## Error Handling Excellence

- Wrap errors with context: `fmt.Errorf("users.create: %w", err)`
- Use sentinel errors for known conditions: `var ErrNotFound = errors.New("not found")`
- Handle errors at the appropriate level
- Never discard errors with `_`

---

## Performance Optimization

- Use `pprof` for profiling
- Benchmark critical paths
- Pre-allocate slices: `make([]int, 0, 1000)`
- Use `sync.Pool` for object reuse
- Understand escape analysis