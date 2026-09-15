---
name: go-backend-testing
description: Use when writing, reviewing, or planning tests for Go backend projects (domain-service-repository-handler architecture, cmd/internal layout). Also use when designing testable code structure before implementation.
---

# Go Backend Testing — Best Practices

## Core Principle

**Test behavior, not implementation.**

If you refactor HOW code works but the outcome stays the same, the test
must still pass. A test that breaks from a pure refactor (rename,
reorder, restructure with no behavior change) is testing implementation,
not behavior — fix the test, not the code.

**Corollary:** if you never watched a test fail for the right reason
(either before the fix, or by deliberately breaking the code), you don't
know if it tests anything real.

## When to Use This Skill

- Writing new business logic (`internal/domain`, `internal/service`)
- Writing or modifying an HTTP handler (`internal/handler/http`)
- Adding a repository method (`internal/repository`)
- Reviewing an existing `_test.go` file for quality
- Deciding test strategy before implementing a feature

## Test-First vs Test-Required (pragmatic stance)

Not strict Iron-Law TDD. Tests are required before code is considered
*done* — not necessarily before code is *written*.

| Code area | Discipline |
|---|---|
| `internal/domain` (pure logic: validation, calculation, normalization) | **Test-first strongly recommended.** Cheapest place to write tests, and where money/data bugs live (e.g. amount normalization). |
| `internal/service` (business logic, orchestration) | Test-first recommended, test-required non-negotiable. |
| `internal/handler/http` | Test-required. Test-first optional. |
| `internal/config`, `internal/logger`, `cmd/api` (wiring) | No test required — see "What NOT to Test" below. |

If prototyping to validate an idea, skip tests until the design settles,
then backfill before merging.

## Architecture Assumption (matches your boilerplate)

```
cmd/api/              → main.go, wiring only (constructs concrete deps, injects into handlers)
internal/handler/http/ → HTTP layer. Parses request, calls service, writes response.
internal/service/      → Business logic. Depends on repository INTERFACE, not concrete DB.
internal/repository/   → interface + concrete impl (e.g. postgres) + you add a fake/mock impl for tests
internal/domain/       → Pure types + pure functions. NO imports from database/sql, net/http, or any driver.
internal/config/       → env/config loading — glue, not tested in depth
internal/middleware/   → cross-cutting HTTP concerns (auth, logging) — test like handlers
internal/logger/       → infra glue — not tested in depth
internal/utils/        → pure helper functions — same rules as domain/
```

**Dependency Rule:** `internal/domain` and `internal/service` must never
import a driver package (`database/sql`, `pgx`, your HTTP framework)
directly. They depend on an **interface** defined in the same package
that needs it (Go idiom: define the interface where it's *consumed*, not
where it's implemented). Concrete implementations live in
`internal/repository` and are wired only in `cmd/api/main.go`.
Testability is the side effect of this rule, not the goal.

**Don't over-layer.** No separate use-case layer, no generic repository
abstraction beyond what `internal/service` actually needs — until a
service genuinely outgrows a handful of unrelated responsibilities.

---

## 1. Unit Testing Pure Functions/Logic (`internal/domain`, `internal/utils`)

Every pure function needs a minimum of 3 categories, expressed as
**table-driven test cases** — this is the idiomatic Go shape and maps
directly onto the 3-category rule:

```go
func TestIsValidSlug(t *testing.T) {
    tests := []struct {
        name string
        slug string
        want bool
    }{
        {"valid slug", "my-link", true},        // happy path
        {"too short", "ab", false},              // boundary
        {"contains whitespace", "my link", false}, // error case
    }

    for _, tt := range tests {
        t.Run(tt.name, func(t *testing.T) {
            got := IsValidSlug(tt.slug)
            if got != tt.want {
                t.Errorf("IsValidSlug(%q) = %v, want %v", tt.slug, got, tt.want)
            }
        })
    }
}
```

**Any `if`, `<`, `<=`, `>=`, `>`, `||`, `&&` in the code under test is a
signal a boundary row is owed in the table.**

Prefer `github.com/stretchr/testify/assert` or `require` over raw
`t.Errorf` once assertions get repetitive — but raw comparisons are
fine and dependency-free for simple cases. Use `require` (not `assert`)
when a failed check should stop the test immediately (e.g. a nil check
before dereferencing).

---

## 2. Naming the Break (gate before writing any test)

Before writing a test case, answer: **what production change would make
this test fail — and is that change a bug or just a decision?**

```
Cannot name a production change that breaks it → redesign around an
  observable behavior instead.
Only an intentional decision breaks it (an exact error string, a
  private struct field) → it's a change detector. Test the behavior
  that depends on the decision instead.
```

**Derive expected values independently — never with the code under
test.**

```go
// ❌ mirror assertion
expected := ToMinorUnit(150.75)
assert.Equal(t, expected, ToMinorUnit(150.75))

// ✅ hand-derived literal
assert.Equal(t, int64(15075), ToMinorUnit(150.75))
```

---

## 3. Mocking Discipline (`internal/service` layer)

**Dependency Injection via interfaces is mandatory.** Define the
repository interface in the package that consumes it (`internal/service`
or `internal/domain`), not in `internal/repository`:

```go
// internal/service/link_service.go
type LinkRepository interface {
    FindBySlug(ctx context.Context, slug string) (*Link, error)
    Save(ctx context.Context, link *Link) error
}

type LinkService struct {
    repo LinkRepository
}
```

`internal/repository` provides the concrete implementation (Postgres,
etc.) that satisfies this interface — Go's structural typing means it
doesn't need to explicitly declare it.

**Mock generation:** use `go.uber.org/mock/gomock` (generates mocks from
interfaces via `mockgen`) or write a small hand-rolled fake struct for
simple interfaces — for 1-3 methods, a hand-rolled fake is often
simpler and more readable than generated mock boilerplate:

```go
// Hand-rolled fake — fine for small interfaces
type fakeLinkRepository struct {
    links map[string]*Link
}

func (f *fakeLinkRepository) FindBySlug(ctx context.Context, slug string) (*Link, error) {
    l, ok := f.links[slug]
    if !ok {
        return nil, nil
    }
    return l, nil
}

func (f *fakeLinkRepository) Save(ctx context.Context, link *Link) error {
    f.links[link.Slug] = link
    return nil
}
```

**Mock only external dependencies. Never mock the logic under test.**
If you mock `LinkService.CreateLink` while testing `CreateLink`, the
test verifies nothing.

**List the real side effects before mocking; mock only the
slow/external part.** A hand-rolled in-memory fake (above) is usually
better than a generated mock here, because the collision-check logic
actually runs against real map lookups instead of pre-programmed mock
responses.

**Never assert on the mock itself as if it were the behavior.** A mock
call assertion (`mockRepo.AssertCalled(t, "Save", ...)`, or checking a
call counter on a hand-rolled fake) is valid ONLY to prove a required
side effect happened, or a forbidden one didn't — never as a substitute
for asserting the real output.

### Verifying side effects, not just results

```go
func TestCreateLink_RejectsInvalidURL_WithoutSaving(t *testing.T) {
    repo := &fakeLinkRepository{links: map[string]*Link{}}
    svc := &LinkService{repo: repo}

    _, err := svc.CreateLink(context.Background(), "not-a-url", "promo")

    assert.Error(t, err)
    assert.Empty(t, repo.links) // proves Save was never called — catches "validates too late" bugs
}
```

---

## 4. Layered Test Strategy

| Layer | Test type | Tool | What it proves |
|---|---|---|---|
| `internal/domain`, `internal/utils` | Unit | `go test` (table-driven) | Pure logic correctness |
| `internal/service` (fake repo) | Unit | `go test` + hand-rolled fake or `gomock` | Business logic branches |
| `internal/repository` | Integration | `sqlmock` (driver-level mock) for query-shape checks, OR `testcontainers-go` (real Postgres in Docker) for constraint/behavior checks | Query correctness, DB constraints, schema assumptions actually hold |
| `internal/handler/http` | Unit / light integration | `net/http/httptest` (`httptest.NewRecorder`, `httptest.NewServer`) | Full request→response wiring without a real running server |
| `cmd/api` end-to-end | Optional E2E | Real binary + `testcontainers-go` for a full-stack smoke test | One critical happy-path flow only |

**`sqlmock` vs `testcontainers-go` — when to use which:**
- `sqlmock` (`github.com/DATA-DOG/go-sqlmock`): mocks the `database/sql`
  driver itself. Fast, no Docker needed. Good for asserting the SQL
  query shape/args are correct. Does **not** validate actual constraint
  behavior (a `sqlmock` test can't tell you if your `CHECK` constraint
  really rejects negative amounts).
- `testcontainers-go`: spins up a real Postgres container. Slower, but
  validates real constraint enforcement, real query execution, real
  type coercion. Use this for endpoints touching money/critical data —
  same principle as "defense in depth" below.

**Proportion:** most tests at the unit level (cheap, `go test` runs in
milliseconds per package), some integration with `testcontainers-go`
(DB constraints matter), very few full E2E.

**Defense in depth is not duplication.** Validating `amount > 0` in
`internal/service` AND as a `CHECK` constraint in the DB is intentional
— the service check can be bypassed by another caller (a cron job, a
migration script); the DB constraint is the last line of defense and
needs its own `testcontainers-go` test.

---

## 5. Concurrency, Context, and Time

Go gives you tools JS doesn't have — use them.

**`context.Context` for cancellation/timeout**, not manual sleep
injection. Any function doing I/O should accept `ctx context.Context`
as its first argument — this is what you inject in tests to control
timeout/cancellation behavior deterministically:

```go
func TestFetchWithTimeout_ReturnsErrorOnDeadlineExceeded(t *testing.T) {
    ctx, cancel := context.WithTimeout(context.Background(), 10*time.Millisecond)
    defer cancel()

    slowFetch := func(ctx context.Context) (string, error) {
        select {
        case <-time.After(100 * time.Millisecond):
            return "data", nil
        case <-ctx.Done():
            return "", ctx.Err()
        }
    }

    _, err := slowFetch(ctx)
    assert.ErrorIs(t, err, context.DeadlineExceeded)
}
```

**For business-logic time dependency** (expiry, staleness), inject
`time.Time` as a parameter rather than calling `time.Now()` inside the
function — same principle as Bun, different syntax:

```go
func IsExpired(createdAt time.Time, now time.Time) bool {
    return now.Sub(createdAt) > 30*24*time.Hour
}
```

Fix `now` to an exact boundary value in tests (e.g. exactly 30 days) —
`time.Now()` changing every run makes boundary tests unreliable.

**Go's race detector is a first-class tool — use it.**
```bash
go test -race ./...
```
This catches real data races (concurrent map writes, unsynchronized
shared state) that a hand-written concurrent test might miss. Run it in
CI, not just locally. It's not a substitute for correct synchronization
design, but it catches what manual review won't.

**Race conditions are still a design problem, not purely a test
problem.** A `sync.WaitGroup`-based test with two goroutines calling
`CreateLink` concurrently can document a race condition risk, but a
passing test is not proof the race is fixed — a fake repo without real
locking may happen to serialize calls. The real defense is atomic DB
constraints (`UNIQUE`, `PRIMARY KEY`) or `SELECT ... FOR UPDATE` /
proper transaction isolation, verified with `testcontainers-go`, not
application-level check-then-act logic.

---

## 6. The Mutation Check (run before marking a test file done)

Mentally mutate the production code. At least one test must fail for
each realistic mutation:

- Wrong constant or argument
- Wrong branch taken (inverted condition, wrong comparison operator)
- Missing side effect / state change
- Zero value returned where a real value is expected (classic Go trap:
  an unchecked `error` return masking a zero-value result)
- Missing validation for zero, empty, negative, or nil input

If a mutation exists that nothing catches, either the behavior is
unprotected (add a test) or the test is tautological (fix it).

**Go-specific trap to check for:** an ignored `error` return
(`result, _ := doSomething()`) is a mutation that silently passes most
tests. If your test only checks the happy-path result and never
constructs a scenario where the error path fires, that's a gap the
mutation check should catch.

---

## 7. What NOT to Test

- **Wiring code** (`cmd/api/main.go` — constructs deps, calls
  `http.ListenAndServe`) — no branches, nothing to verify beyond "does
  it start" (a smoke test/deploy will tell you).
- **Third-party library internals** — don't test that your router (e.g.
  chi, gin) dispatches to the right handler; that's the router's test
  suite. Test the contract at YOUR boundary (the route you registered,
  the payload you produce).
- **`internal/config`, `internal/logger`** — thin glue over env vars/log
  libraries. Test only if there's real parsing/validation logic
  (e.g. a required env var check).
- **Throwaway prototypes** — write tests once the design settles.
- **Trivial struct constructors, getters** — unless they validate,
  default, or have a side effect.

## Coverage — Use as a Radar, Not a KPI

```bash
go test -cover ./...
go test -coverprofile=coverage.out ./... && go tool cover -html=coverage.out
```

- Line coverage can hit 100% while a test never calls `assert`/checks
  nothing — coverage proves the line *executed*, not that the result
  was *verified*.
- A combined condition (`a && b`, `a || b`) needs every combination as a
  table row — line coverage alone won't reveal an uncovered branch.
- Low coverage in `internal/domain`/`internal/service` = red flag. Low
  coverage in `internal/config`/`cmd/api` = usually fine. Never target
  100% — it incentivizes hollow tests.

## Avoiding Brittle Tests

Assert only what the test's stated behavior needs.

```go
// ❌ brittle — breaks if a new field is added to Link
assert.Equal(t, &Link{Slug: "promo", URL: "https://a.com", CreatedAt: someTime}, result)

// ✅ asserts only what this test is about
assert.Equal(t, "promo", result.Slug)
assert.Equal(t, "https://a.com", result.URL)
```

---

## Verification Checklist (before calling a feature "tested")

- [ ] Every pure function/method has happy path + boundary + error case
      (as table rows)
- [ ] Can name the production change that would break each test case
- [ ] No expected value derived using the code under test itself
- [ ] Repository interfaces defined where consumed (service/domain), not
      where implemented
- [ ] Mocks/fakes used only for external dependencies, never for logic
      under test
- [ ] No assertion checks the mock/fake call itself as the behavior
      being verified (only as proof of a required/forbidden side effect)
- [ ] Service tests check both result AND forbidden side effects
- [ ] At least one `testcontainers-go` integration test hits real DB
      constraints for money/critical-data endpoints
- [ ] I/O functions accept `context.Context`; time-dependent logic takes
      `time.Time` as a parameter, not a direct `time.Now()` call
- [ ] `go test -race ./...` passes clean
- [ ] Mutation check passed, including unchecked-error-return trap
- [ ] No test would break from a pure refactor with unchanged behavior

## When Stuck

| Problem | Likely cause | Fix |
|---|---|---|
| Must mock everything to test a function | Code too coupled to a concrete driver/package | Define an interface, inject it (DI) instead of importing the driver directly |
| Table grows unwieldy / setup is huge | Design too complex | Extract helpers; if still complex, simplify the function's responsibility |
| `go test -race` flags something you didn't expect | Real unsynchronized shared state | Don't suppress it — add proper locking or make the state per-goroutine |
| Test passes without exercising real logic | Over-mocked with generated mocks | Switch to a hand-rolled in-memory fake, or an integration test with `testcontainers-go` |