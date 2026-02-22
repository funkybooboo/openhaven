# Testing

Tests are not a checkbox. They are:

- **Living documentation** of how the system behaves
- **A safety net** that lets you change things confidently
- **The first proof** that something works
- **A design tool** -- hard-to-test code is usually poorly designed

---

## Core Principles

### Test behavior, not implementation

Test what a thing does, not how it does it internally. Tests that depend on internal structure break during refactoring -- which is exactly when you need them most.

```
# Bad -- tests that a specific method was called
test "save is called once" {
  mock = MockRepository()
  service.create(data, repo: mock)
  assert mock.save_call_count == 1
}

# Good -- tests the observable result
test "created item is retrievable" {
  repo = InMemoryRepository()
  service.create(data, repo: repo)
  assert repo.find_by_id(data.id) == data
}
```

### No mocks

Mocks test that you called a function with certain arguments. They don't test that the system actually works. They also couple tests to implementation, making refactoring painful.

**Instead of mocks, use:**

- **In-memory implementations** for storage, queues, caches
- **Test adapters** for external services (recording, replaying, or no-op)
- **Real services in containers** for integration tests
- **Controlled implementations** for time, randomness, and other non-determinism

```
# Bad -- mock that tests implementation
mock_storage = Mock()
service = UserService(storage: mock_storage)
service.create(data)
assert mock_storage.save_was_called_with(data)  -- tests wiring, not behavior

# Good -- real in-memory implementation
storage = InMemoryUserStorage()
service = UserService(storage: storage)
service.create(data)
assert storage.find_by_email(data.email) != null  -- tests actual behavior
```

The only acceptable use of test doubles is for external services you don't control -- payment processors, email providers, third-party APIs. Even then, create a **test adapter** that records calls or returns canned responses, rather than a mock.

### Test first

Write tests before or alongside implementation. The red-green-refactor cycle:

1. **Red** -- write a failing test that defines the desired behavior
2. **Green** -- write the minimum code to make it pass
3. **Refactor** -- improve the code while keeping tests green

Tests written after the fact tend to test what the code does. Tests written first define what the code *should* do.

### Deterministic tests

Tests must produce the same result every time, in any order, on any machine.

- No shared mutable state between tests
- No dependency on test execution order
- No dependency on the current time -- use a controlled clock
- No dependency on random values -- seed or control randomness
- No dependency on external services in unit tests

---

## The Testing Pyramid

```
        /\
       /  \   End-to-End (10%)
      /    \  Full system, maps to acceptance criteria
     /------\
    /        \  Integration (20%)
   /          \  Real dependencies, data flows across layers
  /------------\
 /              \  Unit (70%)
/                \  Pure logic, fast, no I/O
```

### Unit tests -- 70%

**What:** Pure functions, business rules, data transformations, validation logic.

**How:** No I/O, no network, no filesystem. Inputs in, outputs out.

**Speed:** The entire unit suite should run in seconds.

**Rule:** If you need a mock to write a unit test, the function has a hidden dependency that should be made explicit.

```
test "applies member discount" {
  // Arrange
  user  = User(type: "member", verified: true)
  price = 100

  // Act
  result = calculate_discount(price, user)

  // Assert
  assert result.final_price  == 90
  assert result.discount_type == "member"
}
```

### Integration tests -- 20%

**What:** Multiple components working together with real dependencies -- storage, file systems, message queues.

**How:** Use real implementations. For databases, use a container that starts fresh for each test run and is cleaned between tests.

**Speed:** Should complete in under a minute.

**Rule:** Each test sets up its own data and cleans up after itself. No test depends on state left by another.

```
test "user is persisted and retrievable" {
  // Arrange
  repo = RealUserRepository(test_database)
  user = User(email: "test@example.com", name: "Test")

  // Act
  saved     = repo.save(user)
  retrieved = repo.find_by_id(saved.id)

  // Assert
  assert retrieved.email == user.email
  assert retrieved.name  == user.name
}
```

### End-to-end tests -- 10%

**What:** Complete workflows from entry point to observable output. Maps directly to acceptance criteria.

**How:** Drive the system through its real interface -- CLI commands, HTTP requests, UI interactions, hardware signals.

**Speed:** Should complete in under 5 minutes.

**Rule:** Every acceptance criterion in every user story maps to at least one E2E test.

---

## Test Structure: Arrange-Act-Assert

Every test follows the same three-part structure:

```
test "filter returns only active items" {
  // Arrange -- set up the preconditions
  items = [
    Item(id: 1, status: "active"),
    Item(id: 2, status: "inactive"),
    Item(id: 3, status: "active"),
  ]

  // Act -- perform the operation being tested
  result = filter_active(items)

  // Assert -- verify the outcome
  assert result.length == 2
  assert all items in result have status "active"
}
```

Keep each section clearly separated. If Arrange is very long, extract a factory or builder helper.

---

## Test Naming

Test names describe behavior. A failing test name should tell you exactly what broke without reading the test body.

```
# Bad -- describes implementation
test_filter_method
test_save_called
test_returns_true

# Good -- describes behavior
test_filter_returns_only_active_items
test_created_user_is_retrievable_by_email
test_invalid_email_raises_validation_error
test_expired_token_is_rejected
test_empty_input_returns_empty_result
```

---

## Test Independence

- Each test sets up its own state
- Each test cleans up after itself (or uses isolated resources)
- Tests pass in any order
- Tests pass in isolation
- Tests pass in parallel

---

## Coverage

Coverage is a tool, not a goal. 100% coverage with bad tests is worse than 80% coverage with good tests.

- All critical business logic must be covered
- All error paths must be covered
- Aim for 85%+ overall
- Don't write tests just to hit a number

---

## Running Tests

```bash
task test           # all tests
task test:watch     # watch mode -- re-runs on file change
task test:coverage  # with coverage report
```

Configure additional test commands in `Taskfile.yml` for your project's needs.
