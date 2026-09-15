---
name: bun-backend-testing
description: Use when writing, reviewing, or planning tests for Bun/TypeScript backend projects (Hono, REST APIs, Cloudflare Workers, domain-service-repository architecture). Also use when designing testable code structure before implementation.
---

# Bun Backend Testing — Best Practices

## Core Principle

**Test behavior, not implementation.**

If you refactor HOW code works but the outcome stays the same, the test
must still pass. If a test breaks from a pure refactor (rename, reorder,
restructure with no behavior change), the test is wrong — it is testing
implementation, not behavior.

**Corollary:** if you didn't watch a test fail for the right reason at
least once (either before writing the fix, or by deliberately breaking
the code after), you don't know if it tests anything real.

## When to Use This Skill

- Writing new business logic (domain / service layer)
- Writing or modifying a REST API endpoint (Hono handler)
- Adding a repository / DB access method
- Reviewing an existing test file for quality
- Deciding test strategy before implementing a feature

## Test-First vs Test-Required (pragmatic stance)

This is **not** strict Iron-Law TDD. Tests are required before code is
considered *done* — not necessarily before code is *written*.

| Code area | Discipline |
|---|---|
| `domain/` (pure functions: validation, calculation, normalization) | **Test-first strongly recommended.** This is where money/data bugs live (e.g. minor-unit amount normalization). Writing the test first here costs almost nothing and forces you to think through boundaries before coding. |
| `service/` (business logic, orchestration) | Test-first recommended, test-required non-negotiable. |
| `handler/` (Hono routes) | Test-required. Test-first optional. |
| Glue code, wiring, `index.ts`, prototyping/exploration | No test required. Don't waste time here (see "When NOT to test" below). |

If you're prototyping to validate an idea (tracer bullet), skip tests
entirely until the design settles — then backfill before merging.

## Architecture Assumption

```
handler/   → Hono route. Parses request, calls service, formats response.
service/   → Business logic. Depends on repository INTERFACE, not concrete DB.
repository/→ interface (types.ts) + concrete impl (d1.ts) + fake impl (memory.ts)
domain/    → Pure functions. NO imports from Hono, D1, or any framework.
```

**Dependency Rule:** `domain/` and `service/` must never import a
framework or infra package (Hono, D1, fetch, Date.now directly). They
depend on interfaces; concrete implementations are wired only in
`index.ts`. This is not a testing rule — it's an architecture rule that
*happens* to make testing easy. Testability is the side effect, not the
goal.

**Don't over-layer.** No separate use-case layer, no DTO mapper, no
repository abstraction beyond one interface — until `service/` actually
grows past a handful of unrelated use-cases. Three layers is enough at
solo-founder scale.

---

## 1. Unit Testing Pure Functions (`domain/`)

Every pure function needs a minimum of 3 categories of test:

1. **Happy path** — normal expected input.
2. **Boundary** — zero, empty, exact-equal, max value, min valid value.
   Any `if`, `<`, `<=`, `>=`, `>`, `||`, `&&` in the code is a signal a
   boundary test is owed.
3. **Error case** — invalid input that must be rejected.

```typescript
describe("isValidSlug", () => {
  test("accepts a valid slug", () => {
    expect(isValidSlug("my-link")).toBe(true);
  });

  test("rejects slug shorter than 3 chars", () => {   // boundary
    expect(isValidSlug("ab")).toBe(false);
  });

  test("rejects slug with whitespace", () => {         // error case
    expect(isValidSlug("my link")).toBe(false);
  });
});
```

**Assertion choice:** `toBe()` for primitives (identity = value).
`toEqual()` for objects/arrays (deep value comparison) — using `toBe()`
on a freshly-returned object always fails regardless of correctness.

**AAA structure** (Arrange-Act-Assert) — collapse to one line for
trivial tests, expand with named variables + comments once setup has
more than one step.

---

## 2. Naming the Break (gate before writing any test)

Before writing a test body, answer: **what production change would make
this test fail — and is that change a bug or just a decision?**

```
Cannot name a production change that breaks it → redesign around an
  observable behavior instead.
Only an intentional decision breaks it (a constant's exact value,
  private structure) → it's a change detector. Test the behavior that
  depends on the decision instead.
The "break" is just the source text changing → you're asserting on
  text, not behavior. Run the code and assert its output/effects.
```

**Derive expected values independently.** Never compute the expected
value using the same function/helper under test — that's a mirror
assertion, always true regardless of correctness.

```typescript
// ❌ mirror assertion
const expected = toMinorUnit(150.75);
expect(toMinorUnit(150.75)).toBe(expected);

// ✅ hand-derived literal
expect(toMinorUnit(150.75)).toBe(15075n);
```

---

## 3. Mocking Discipline (`service/` layer)

**Dependency Injection is mandatory** for anything external (repository,
HTTP client, sleep, current time). This is how the service layer stays
unit-testable without touching D1/network/clock.

**Mock only external dependencies.** Never mock the logic you're
actually testing — if you mock `service.createTransaction` while
testing `service.createTransaction`, the test verifies nothing.

**Before mocking anything, list its real side effects and mock only the
slow/external part** — keep everything the test depends on real.

```typescript
// ❌ mocks away the exact thing that matters (slug collision logic)
const mockRepo = { findBySlug: mock(async () => undefined) };

// ✅ real in-memory repo — collision logic actually runs
const repo = createInMemoryLinkRepository();
```

**Mirror the real shape completely** when a mock is unavoidable (e.g.
external payment API). A partial mock passes tests while production
breaks on a field the mock omitted.

**Never assert on the mock itself.** A mock assertion (`expect(mockFn)
.toHaveBeenCalled()`) is valid ONLY to prove a required side effect
happened or a forbidden one didn't (see next section) — never as a
substitute for asserting real behavior/output.

**When mock setup outgrows the test logic**, or you can't explain why a
mock is needed → switch to an integration test with the real component
(in-memory repo, SQLite in-memory) instead.

### Verifying side effects, not just results

Controller/service tests must check both:
1. The returned result / response is correct.
2. A side effect that SHOULD NOT happen on the failure path actually
   didn't (`not.toHaveBeenCalled()`).

```typescript
test("rejects amount <= 0 without touching the service", async () => {
  const result = await createTransactionHandler(
    { userId: "u1", body: { amount: 0 } },
    mockService
  );

  expect(result.status).toBe(400);
  expect(mockService.createTransaction).not.toHaveBeenCalled(); // catches "validates too late" bugs
});
```

---

## 4. Layered Test Strategy (REST API)

| Layer | Test type | Tool | What it proves |
|---|---|---|---|
| `domain/` | Unit | `bun:test` | Pure logic correctness |
| `service/` (mocked repo) | Unit | `bun:test` + fake/mock repo | Business logic branches |
| `service/` (real repo) | Integration | `bun:test` + SQLite in-memory (`bun:sqlite`, `:memory:`) or in-memory repo fake | DB constraints, query correctness, schema/type assumptions actually hold |
| `handler/` | Unit/light E2E | `bun:test` + Hono `app.request()` | Full request→response wiring, no real server needed |
| Real D1 on Workers runtime | Integration (optional) | `@cloudflare/vitest-pool-workers` (separate from `bun:test`) | Only when D1-specific behavior is in question — don't reach for this by default |

**Proportion:** most tests at the unit level (cheap), some integration
(DB constraints matter — e.g. `PRIMARY KEY` collision, `CHECK` on
amount > 0), very few full E2E (one critical happy-path flow is enough).

**Defense in depth is not duplication.** Validating `amount > 0` in
both the service AND the DB `CHECK` constraint is intentional — the
service check can be bypassed by another caller (worker, cron); the DB
constraint is the last line of defense and needs its own test.

---

## 5. Async & Time

Never hardcode `setTimeout` or `Date.now()` inside logic that needs
testing. Inject both, defaulting to the real implementation:

```typescript
export async function fetchWithRetry(
  fetchFn: () => Promise<unknown>,
  maxRetries: number,
  delayMs: number,
  sleep: (ms: number) => Promise<void> = (ms) => new Promise(r => setTimeout(r, ms))
) { /* ... */ }
```

In tests, inject a mock `sleep` that resolves instantly — verify it was
*called* with the right delay, without ever waiting for it.

For time-dependent logic (expiry, staleness), inject `now` the same way
and fix it to an exact boundary value in tests (e.g. exactly 30 days) —
`Date.now()` changing every run makes boundary tests unreliable.

**Race conditions are a design problem, not a test problem.** A test
using `Promise.allSettled` on two concurrent calls can document a race
condition risk, but mocked dependencies resolve too fast/synchronously
to reliably reproduce it — a passing test here is NOT proof the race is
fixed. The real defense is atomic DB constraints (`PRIMARY KEY`, unique
index, transaction isolation), not application-level check-then-act
logic.

---

## 6. The Mutation Check (run before marking a test file done)

Mentally mutate the production code. At least one test must fail for
each realistic mutation:

- Wrong constant or argument
- Wrong branch taken
- Missing side effect / state change
- Empty or default return where a real value is expected
- Missing validation for zero, empty, negative, or malformed input

If a mutation exists that nothing catches, either the behavior is
unprotected (add a test) or the test is tautological (fix it).

---

## 7. What NOT to Test

- **Glue/wiring code** (`index.ts` that just constructs and injects) —
  no branches, nothing to verify beyond "does it run" (deploy will tell
  you).
- **Third-party library internals** (don't test that Hono routes a
  request — that's Hono's test suite's job). Test the contract at YOUR
  boundary (the route you registered, the payload you produce).
- **Throwaway prototypes / exploration code** — write tests once the
  design settles, not while still validating the idea.
- **Trivial constructors, getters, pure forwarding** — unless they
  validate, normalize, default, or have a side effect.

## Coverage — Use as a Radar, Not a KPI

- Line coverage can hit 100% while a test asserts nothing (`expect()`
  never called) — coverage only proves the line *executed*, not that the
  result was *verified*.
- Prefer branch coverage over line coverage — a combined condition
  (`a && b`, `a || b`) needs every combination tested, not just one path
  through it.
- Low coverage in business logic / validation = red flag. Low coverage
  in glue code = irrelevant. Never make "100% coverage" a target — it
  incentivizes hollow tests.

## Avoiding Brittle Tests

Assert only what the test's stated behavior needs — not the entire
shape of a response object. A test that breaks when an unrelated field
is added is testing structure, not behavior.

```typescript
// ❌ brittle — breaks if a new field is added to the response
expect(result).toEqual({ slug: "promo", url: "...", createdAt: expect.any(String) });

// ✅ asserts only what this test is about
expect(result.slug).toBe("promo");
expect(result.url).toBe("https://example.com");
```

---

## Verification Checklist (before calling a feature "tested")

- [ ] Every pure function has happy path + boundary + error case
- [ ] Can name the production change that would break each test
- [ ] No expected value derived using the code under test itself
- [ ] Mocks used only for external dependencies, never for logic under test
- [ ] Mock responses mirror the real shape completely
- [ ] No assertion checks the mock itself as the behavior being verified
- [ ] Controller tests check both result AND forbidden side effects
- [ ] At least one integration test hits real DB constraints for
      money/critical-data endpoints
- [ ] Async code has injectable time/delay, no hardcoded `setTimeout`/`Date.now()`
- [ ] Mutation check passed — no silent, uncaught mutation
- [ ] No test would break from a pure refactor with unchanged behavior

## When Stuck

| Problem | Likely cause | Fix |
|---|---|---|
| Must mock everything to test a function | Code too coupled to infra | Inject the dependency (DI) instead of hardcoding it |
| Test setup is huge | Design too complex | Extract helpers; if still complex, simplify the interface |
| Can't tell why a test failed | Test doing too much | Split into one-behavior-per-test |
| Test passes without any real logic exercised | Over-mocked | Mock fewer layers; use a real in-memory fake instead |