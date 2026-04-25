# Testing Standards (Cross-Language)

| Focus Area | Standard |
| :--- | :--- |
| **Philosophy** | Test pyramid, AAA pattern |
| **Coverage** | Unit: 80% min, Branch: 75% min |
| **Naming** | `test_<unit>_<scenario>_<expected_result>` |
| **Structure** | Arrange-Act-Assert (AAA) |

---

## Test Pyramid

```
        /\
       /  \      E2E Tests (Few)
      /----\     - Critical user journeys
     /      \    - Smoke tests
    /--------\   Integration Tests (Some)
   /          \  - API contracts
  /------------\ - Database interactions
 /              \ Unit Tests (Many)
/________________\
                  - Business logic
                  - Pure functions
```

---

## Guiding Principles

| Principle | Description |
| :--- | :--- |
| **Test behavior, not implementation** | Tests should verify what code does, not how |
| **Arrange-Act-Assert (AAA)** | Clear structure in every test |
| **One assertion per concept** | Tests should fail for one reason |
| **Deterministic** | No flaky tests; mock external dependencies |
| **Fast** | Unit tests should run in milliseconds |
| **Independent** | Tests should not depend on each other |

---

## Coverage Requirements

| Type | Minimum | Target |
| :--- | --- | --- |
| Unit | 80% | 90% |
| Branch | 75% | 85% |
| Integration | Critical paths | All APIs |
| E2E | Happy paths | Key journeys |

### What to Cover

- Business logic and domain rules
- Edge cases and boundary conditions
- Error handling paths
- Input validation

### What NOT to Cover

- Third-party library internals
- Simple getters/setters
- Framework boilerplate
- Generated code

---

## Test Naming Convention

### Format

```
test_<unit>_<scenario>_<expected_result>
```

### Examples

```go
// Go
func TestCalculateTotal_WithDiscount_ReturnsReducedPrice(t *testing.T) {}
func TestUserLogin_InvalidPassword_ReturnsAuthError(t *testing.T) {}
func TestOrderSubmit_WhenCartEmpty_ReturnsValidationError(t *testing.T) {}
```

```python
# Python
def test_calculate_total_with_discount_returns_reduced_price():
def test_user_login_with_invalid_password_raises_auth_error():
def test_order_submit_when_cart_empty_returns_validation_error():
```

```csharp
// C#
[Fact]
public void CalculateTotal_WithDiscount_ReturnsReducedPrice() { }
[Fact]
public void UserLogin_InvalidPassword_ThrowsAuthError() { }
```

---

## Test Structure (AAA Pattern)

### Arrange-Act-Assert

```go
func TestUserCreate(t *testing.T) {
    // Arrange
    repo := NewMockUserRepository()
    service := NewUserService(repo)
    input := CreateUserInput{Name: "Alice", Email: "alice@example.com"}

    // Act
    user, err := service.Create(context.Background(), input)

    // Assert
    require.NoError(t, err)
    assert.Equal(t, "Alice", user.Name)
    assert.Equal(t, "alice@example.com", user.Email)
    repo.SaveCalledOnce()
}
```

### Test Doubles

| Type | Purpose | Example |
| :--- | :--- | :--- |
| **Stub** | Provide canned answers | `mock.Returns(nil, ErrNotFound)` |
| **Mock** | Verify interactions | `mock.AssertCalled(t, "Save")` |
| **Fake** | Working implementation | In-memory database |
| **Spy** | Record calls, use real impl | `spy.On(method).Return(result)` |

---

## Unit Testing

### Characteristics

- Tests single unit (function, method, class)
- No external dependencies (DB, network, filesystem)
- Runs in isolation
- Fast (< 100ms per test)

### Mocking Guidelines

```go
// Mock at boundaries, not everywhere
// Good: Mock external service
func TestProcessData(t *testing.T) {
    client := NewMockExternalClient()
    client.On("FetchData", "key").Return(Value: 42, nil)
    
    result := ProcessData(client)
    assert.Equal(t, 42, result)
}

// Bad: Mocking internal implementation details
// DON'T: mock private helpers
```

---

## Integration Testing

### Characteristics

- Tests component interactions
- May use real databases (containerized)
- Tests API contracts
- Slower than unit tests (seconds)

### Database Testing Pattern

```go
// Use transactions for isolation
func withTx(t *testing.T, fn func(tx *sql.Tx)) {
    tx, _ := db.Begin()
    _, _ = tx.Exec("DELETE FROM users") // Clean slate
    fn(tx)
    tx.Rollback()
}
```

---

## Performance Testing

### Benchmarks (Go)

```go
func BenchmarkQuickSort(b *testing.B) {
    data := generateTestData(10000)
    b.ResetTimer()
    for i := 0; i < b.N; i++ {
        QuickSort(data)
    }
}
```

### Run benchmarks

```bash
go test -bench=. -benchmem ./...
```

---

## Test Automation Commands

| Language | Command |
| :--- | :--- |
| **Go** | `go test -race -cover ./...` |
| **C#** | `dotnet test --collect:"Code Coverage"` |
| **Zig** | `zig build test` |
| **Python** | `uv run pytest --cov=src` |

---

## CI/CD Quality Gates

| Stage | Check |
| :--- | :--- |
| **Lint** | `golangci-lint run`, `dotnet format` |
| **Test** | `go test -race -cover ./...` |
| **Build** | `go build ./...` |