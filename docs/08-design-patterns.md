# Design Patterns

Patterns are solutions to recurring problems. Use them when they solve a problem you actually have -- not before. Every pattern adds complexity; make sure it's earning its keep.

The pseudocode here is intentionally language-neutral. Translate it to your language's idioms.

---

## 1. Adapter Pattern

**Problem:** Your core logic needs to talk to the outside world -- a database, a file, a network, a terminal -- but you don't want to couple your logic to a specific implementation.

**Solution:** Define an interface (the port) that describes what you need. Write concrete implementations (adapters) that satisfy it.

```
interface Storage {
  save(record)    -> record
  findById(id)    -> record | null
  findAll()       -> list<record>
  delete(id)      -> void
}

class PostgresStorage implements Storage { ... }  // production
class InMemoryStorage implements Storage { ... }  // tests / dev
class FileStorage implements Storage { ... }      // embedded / offline
```

Your core logic depends only on `Storage`. You can swap the implementation without touching a single line of business logic.

**When to use:** Any time core logic needs to interact with infrastructure -- storage, logging, config, time, external APIs, hardware.

---

## 2. Dependency Injection

**Problem:** Components need collaborators, but hardcoding those collaborators creates tight coupling and makes testing painful.

**Solution:** Pass dependencies in rather than constructing them inside. Wire everything together at the composition root.

```
# Tightly coupled -- hard to test, hard to change
class OrderService {
  constructor() {
    this.storage = new PostgresStorage()  // hardcoded
    this.mailer  = new SmtpMailer()       // hardcoded
  }
}

# Loosely coupled -- easy to test, easy to swap
class OrderService {
  constructor(storage, mailer) {
    this.storage = storage
    this.mailer  = mailer
  }
}

# Composition root wires it together
storage = PostgresStorage(config.databaseUrl)
mailer  = SmtpMailer(config.smtpHost)
service = OrderService(storage, mailer)
```

**When to use:** Always. This is not optional -- it's the foundation of testable, maintainable code.

---

## 3. Repository Pattern

**Problem:** Data access logic is scattered throughout the codebase. Business logic is tangled with queries and I/O.

**Solution:** A repository provides a collection-like interface to your data. Business logic asks the repository for records; the repository handles the details of how they're stored and retrieved.

```
interface UserRepository {
  findById(id)         -> user | null
  findByEmail(email)   -> user | null
  save(user)           -> user
  delete(id)           -> void
}

# Business logic -- no SQL, no file paths, no HTTP
class UserService {
  constructor(users: UserRepository) { ... }

  register(email, name) {
    if users.findByEmail(email) exists -> raise ConflictError
    user = User(id: generateId(), email: email, name: name)
    return users.save(user)
  }
}
```

**When to use:** Any time you have persistent data. The repository is the boundary between your domain and your storage layer.

---

## 4. Result Pattern

**Problem:** Functions that can fail have two options: return a value or raise an exception. Exceptions are invisible in function signatures -- callers don't know a function can fail without reading its implementation.

**Solution:** Return a value that explicitly represents either success or failure. Inspired by Rust's `Result<T, E>`.

```
type Result = Ok(value) | Err(error)

function parseEmail(raw) -> Result {
  if raw does not contain "@" -> return Err(ValidationError("invalid email"))
  return Ok(raw.trim().lowercase())
}

# Caller must handle both cases -- no surprises
result = parseEmail(input)
if result is Ok  -> proceed with result.value
if result is Err -> handle result.error
```

**When to use:** Business logic that can fail in expected, recoverable ways. Not for unexpected errors -- those should still propagate as exceptions.

---

## 5. Strategy Pattern

**Problem:** You have multiple algorithms or behaviors for the same task and need to select between them at runtime.

**Solution:** Define a common interface for the behavior. Implement each variant separately. Select the right one based on context.

```
# Each strategy is a function (or small object) with the same signature
regularPrice(base)  -> base
studentPrice(base)  -> base * 0.90
bulkPrice(base)     -> base * 0.85

# Selector picks the right one
function selectPricing(userType) -> strategy {
  "student" -> studentPrice
  "bulk"    -> bulkPrice
  default   -> regularPrice
}

# Usage
strategy   = selectPricing(user.type)
finalPrice = strategy(basePrice)
```

**When to use:** Multiple interchangeable implementations of the same behavior. Prefer functions over classes for simple strategies.

---

## 6. Decorator Pattern

**Problem:** You want to add cross-cutting behavior -- logging, caching, retry, metrics -- to an existing component without modifying it.

**Solution:** Wrap the component in another component that implements the same interface, adding behavior before or after delegating to the wrapped component.

```
# Base interface
interface Storage { save(r), findById(id), ... }

# Logging decorator -- wraps any Storage
class LoggingStorage implements Storage {
  constructor(inner: Storage, logger: Logger)

  save(record) {
    logger.info("saving", id: record.id)
    result = inner.save(record)
    logger.info("saved",  id: result.id)
    return result
  }
}

# Stack decorators -- each adds one concern
base    = PostgresStorage(connection)
logged  = LoggingStorage(base, logger)
cached  = CachingStorage(logged, ttl: 300)

# Everything downstream sees the same Storage interface
service = RecordService(storage: cached)
```

**When to use:** Cross-cutting concerns like logging, caching, retry logic, rate limiting, metrics. Keeps each concern isolated and composable.

---

## 7. Observer / Event Pattern

**Problem:** When something happens, multiple other parts of the system need to react -- but the thing that happened shouldn't need to know about all its observers.

**Solution:** Emit events. Observers subscribe to the events they care about. The emitter knows nothing about the observers.

```
eventBus.on("user.registered", sendWelcomeEmail)
eventBus.on("user.registered", createDefaultSettings)
eventBus.on("user.registered", logRegistrationMetric)

# Registration logic just emits -- it doesn't know what happens next
function register(email) {
  user = users.save(User(email: email))
  eventBus.emit("user.registered", { userId: user.id, email: user.email })
  return user
}
```

**When to use:** One event should trigger multiple independent reactions. Keeps the emitter decoupled from its effects.

---

## Pattern Selection Guide

| Problem | Pattern |
|---------|---------|
| Core logic needs infrastructure | Adapter |
| Components need collaborators | Dependency Injection |
| Need to abstract data access | Repository |
| Function can fail in expected ways | Result |
| Multiple interchangeable algorithms | Strategy |
| Add behavior without modifying a class | Decorator |
| One event, multiple reactions | Observer |

---

## Anti-Patterns to Avoid

**God object** -- one class that knows everything and does everything. Split it.

**Service locator** -- passing a container around and pulling dependencies from it inside functions. Pass the specific dependency instead.

**Premature abstraction** -- creating an interface before you have two implementations. Wait until you feel the pain.

**Inheritance for code reuse** -- use composition. Inheritance couples you to the parent's implementation details.

**Pattern for pattern's sake** -- every pattern adds complexity. Only add one when it solves a real problem you have right now.
