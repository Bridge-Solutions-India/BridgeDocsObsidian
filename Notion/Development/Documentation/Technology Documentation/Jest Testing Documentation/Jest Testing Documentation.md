---

---

Testing is a process of testing an application to check that it meets the given conditions, standards and best practices - Test Driven Development approach (TDD) 

[[Testing Best Practices]]

Types of Testing

1. [**Unit Testing**](https://www.notion.so/Jest-Testing-Documentation-2d9118601eca80208d24fcfee60320a1#2dc118601eca8002a3a2e3f075afc946)
2. [Integration Testing](/2d9118601eca80208d24fcfee60320a1#2dc118601eca80d5a8c8c1807ec13463)
3. Automation Testing (out of scope for our project)

Key Concepts in Testing

[Mock Testing](https://www.notion.so/Jest-Testing-Documentation-2d9118601eca80208d24fcfee60320a1#2dc118601eca800fbd7dc65e4179645d)

# 🧪 What Is Unit Testing?

**Unit Testing** is the practice of testing the **smallest testable units of code** (functions, methods, classes) **in isolation** to verify that they behave correctly under all relevant conditions.

> A unit is a piece of code with a single responsibility and no external side effects.

## 🎯 Goals of Unit Testing

Unit tests aim to ensure:

✔ Correct business logic

✔ Early bug detection

✔ Safe refactoring

✔ Clear documentation of behavior

✔ Fast feedback during development

## 🔍 What Counts as a “Unit”?

### Backend (Node / Java / Python)

- Functions
- Service methods
- Utility classes
- Validation logic
- Domain rules

### Frontend (React / Angular)

- Pure components
- Reducers
- Hooks
- Utility functions

❌ NOT units:

- Databases
- APIs
- File systems
- Network call

## 🧠 Core Principles of Unit Testing

## 1️⃣ Isolation (Most Important)

Each unit test must run **independently**, without relying on:

- DB
- Network
- File system
- Other tests

✔ Achieved using **mocks and stubs**

---

## 2️⃣ Deterministic Behavior

Unit tests should:

- Always pass or fail the same way
- Not depend on time, randomness, or environment

❌ Avoid:

```javascript
newDate()
Math.random()
```

✔ Use:

```plain text
jest.useFakeTimers();
```

---

## 3️⃣ Fast Execution

- Unit tests should run in **milliseconds**
- Thousands of tests should finish in seconds

> If tests are slow, developers stop running them.

---

## 4️⃣ One Assertion of Behavior

Each test should verify **one logical behavior**, even if it needs multiple assertions.

❌ Bad:

```javascript
it("creates user and sends email and logs activity")
```

✅ Good:

```javascript
it("creates user successfully")
```

---

# 🧱 Unit Testing Structure (AAA Pattern)

### Arrange – Act – Assert

```plain text
it("returns false for invalid email",() => {
// Arrange
const email ="invalid";

// Act
const result =isValidEmail(email);

// Assert
expect(result).toBe(false);
});


```

✔ Clear

✔ Readable

✔ Consistent

## Example:

### 🧪 Example: Unit Test in Node.js (Jest)

Function Under Test

```javascript
functioncalculateDiscount(price) {
if (price <=0)thrownewError("Invalid price");
return price >1000 ? price *0.1 :0;
}
```

---

### Unit Tests

```javascript
describe("calculateDiscount",() => {
it("returns 10% discount for price above 1000",() => {
expect(calculateDiscount(2000)).toBe(200);
  });

it("returns 0 discount for price below 1000",() => {
expect(calculateDiscount(500)).toBe(0);
  });

it("throws error for invalid price",() => {
expect(() =>calculateDiscount(0)).toThrow("Invalid price");
  });
});
```

✔ No DB

✔ No HTTP

✔ Pure logic

## 📏 What to Test in Unit Tests

### Always Test

✔ Business rules

✔ Validation logic

✔ Conditional branches

✔ Edge cases

✔ Error handling

### Avoid Testing

❌ Framework behavior

❌ Library internals

❌ Private methods

## 🧪 Unit Test Coverage Areas

| Area | Example |
| --- | --- |
| Happy Path | Valid input |
| Boundary Conditions | Empty, null, max/min |
| Error Conditions | Exceptions |
| Edge Cases | Unexpected but valid inputs |

## 🏗 Unit Tests in TDD Context

In **TDD**, unit tests:

4. Are written **before** code
5. Define behavior
6. Shape design
7. Protect refactoring

**Flow**

```plain text
Write unit test →Fail →Implement →Pass →Refactor
```

---

# 🧠 Unit Testing Best Practices

## ✔ Keep Tests Simple

- Minimal setup
- Clear assertions

## ✔ Name Tests Clearly

```plain text
it("throws ValidationError when email is missing")
```

## ✔ Avoid Over-Mocking

Mock **only** external boundaries.

## ✔ Test Behavior, Not Implementation

❌ Don’t assert internal function calls

✔ Assert outputs and effects


# 🔗 What Is Integration Testing?

**Integration Testing** verifies that **multiple units or modules work together correctly** when integrated.

> Unlike unit tests (which test logic in isolation), integration tests validate real interactions between components.

---

## 🎯 Purpose of Integration Testing

Integration tests aim to catch problems that **unit tests cannot**, such as:

✔ Incorrect DB queries

✔ API contract mismatches

✔ Middleware order issues

✔ Serialization / deserialization errors

✔ Configuration issues

---

# 🧠 Where Integration Testing Fits (Test Pyramid)

```plain text
           E2ETests(Few)
     IntegrationTests(Some)
  UnitTests(Many)
```

| Layer | Focus |
| --- | --- |
| Unit | Logic correctness |
| Integration | Component collaboration |
| E2E | User workflows |

---

# 🧩 What Is “Integrated” in Integration Tests?

Typically integrates **2–4 real components**, for example:

✔ Controller + Service

✔ Service + Database

✔ API + Middleware

✔ Repository + ORM

❌ Not:

- UI + Browser automation (E2E)
- Single function (Unit)

---

# 🔌 Real vs Mocked Dependencies

### Integration Tests Use:

✔ Real database (or test DB)

✔ Real middleware

✔ Real routing

✔ Real serialization

### Integration Tests May Mock:

❌ External third-party APIs

❌ Email/SMS services

> Rule: Integrate what you own, mock what you don’t.

---

# 🏗 Example: Integration Testing an Express API

### Flow Tested

```plain text
HTTP Request → Middleware → Controller → Service → DB → Response
```

---

## Example Setup (Express + Jest + Supertest)

### Test File

```plain text
describe("POST /contact (integration)",() => {
it("creates a contact with valid data",async () => {
const res =awaitrequest(app)
      .post("/contact")
      .send({
name:"Hari",
email:"hari@test.com",
description:"Hello",
      });

expect(res.status).toBe(201);
expect(res.body.success).toBe(true);
  });
});
```

✔ Real routing

✔ Real validation

✔ Real DB insert

---

# 🧪 Integration Test Database Strategy

### Best Options

### 1️⃣ Separate Test Database (Recommended)

- Same schema
- Same migrations
- Cleaned between tests

```plain text
DB_NAME=app_test
```

---

### 2️⃣ In-Memory DB (If Possible)

- SQLite
- H2 (Java)
- Faster but less realistic

---

### 3️⃣ Transaction Rollback (Advanced)

- Wrap each test in transaction
- Roll back after test

---

# 🧹 Test Isolation & Cleanup (Critical)

Each integration test must start clean.

### Cleanup Methods

```plain text
beforeEach(async () => {
await db.execute("DELETE FROM contacts");
});
```

or

```javascript
afterAll(async () => {
await db.close();
});
```

❌ Never rely on test order

---

# 🧪 What to Assert in Integration Tests

✔ HTTP status codes

✔ Response structure

✔ DB side effects

✔ Error handling

✔ Middleware behavior

❌ Avoid:

- Internal function calls
- Private methods

---

# 🧠 Integration Testing in TDD

Integration tests are usually written:

- **After unit tests**
- Or to validate critical flows

**TDD flow**

8. Unit test drives logic
9. Integration test validates wiring

---

# ⚖️ Unit vs Integration Testing

| Aspect | Unit | Integration |
| --- | --- | --- |
| Speed | Very fast | Slower |
| Isolation | Full | Partial |
| DB | Mocked | Real |
| Coverage | Logic | Wiring |
| Quantity | Many | Few |

---

# 🚦 Error Scenarios in Integration Tests

Integration tests should include:

✔ Invalid payload

✔ Missing headers

✔ Auth failure

✔ DB failure

✔ Middleware errors

Example:

```javascript
it("returns 400 for invalid body",async () => {
const res =awaitrequest(app)
    .post("/contact")
    .send("invalid-json");

expect(res.status).toBe(400);
});
```

---

# 📂 Recommended Folder Structure

```plain text
tests/
  unit/
    services/
  integration/
    controllers/
  e2e/
```

✔ Clear intent

✔ Faster execution

---

# 🧠 Best Practices for Integration Testing

## ✔ Keep Integration Tests Focused

Test **one flow at a time**, not everything.

---

## ✔ Avoid Overuse

Too many integration tests:

- Slow builds
- Hard debugging

---

## ✔ Use Realistic Data

Match production formats & constraints.

---

## ✔ Run Separately in CI

```bash
npm runtest:unit
npm runtest:integration
```

---

# 🚫 Common Integration Testing Mistakes

| Mistake | Why It’s Bad |
| --- | --- |
| Using production DB | Dangerous |
| No cleanup | Flaky tests |
| Testing everything | Slow suite |
| Mixing unit & integration | Confusion |
| Mocking DB | Becomes unit test |

---

# 🏆 Benefits of Integration Testing

✔ Catches real-world issues

✔ Validates system wiring

✔ Increases confidence

✔ Prevents production failures


# 🎭 What Is Mocking?

**Mocking** is the practice of **replacing real dependencies** with **controlled fake implementations** during tests.

> A mock simulates the behavior of a real object and allows you to control outputs and verify interactions.

## 🎯 Why Mocking Is Needed

Without mocks, unit tests become:

- Slow
- Flaky
- Hard to reproduce
- Dependent on external systems

### Mocking helps to:

✔ Isolate the unit under test

✔ Control external behavior

✔ Simulate errors & edge cases

✔ Make tests fast & deterministic

---

# 🔌 What Should Be Mocked?

### Mock These (External Boundaries)

✔ Databases

✔ APIs

✔ File systems

✔ Email / SMS services

✔ Time & randomness

### Do NOT Mock These

❌ Business logic

❌ Validation rules

❌ Pure utility functions

> Golden rule: Mock what you don’t own.

---

# 🧩 Types of Test Doubles (Important Concept)

Mocking is part of a family called **Test Doubles**.

| Type | Purpose |
| --- | --- |
| Dummy | Placeholder, unused |
| Stub | Returns fixed data |
| Mock | Verifies interactions |
| Spy | Wraps real implementation |
| Fake | Lightweight real implementation |

---

## 1️⃣ Dummy

Used only to satisfy parameters.

```plain text
createUser(null,"unused");
```

---

## 2️⃣ Stub

Returns predefined responses.

```plain text
const dbStub = {
findUser:() => ({id:1 }),
};
```

✔ No verification

✔ Just data

---

## 3️⃣ Mock (Most Common)

Simulates behavior **and** verifies calls.

```plain text
const dbMock = {
save: jest.fn().mockResolvedValue({id:1 }),
};

expect(dbMock.save).toHaveBeenCalled();
```

---

## 4️⃣ Spy

Tracks calls to a real method.

```javascript
const spy = jest.spyOn(console,"log");
console.log("hello");

expect(spy).toHaveBeenCalled();
```

✔ Real implementation still runs

---

## 5️⃣ Fake

Simplified real implementation.

```plain text
const fakeCache =newMap();
```

✔ Faster than real DB

✔ Still functional

---

# [🧪 Mocking in Unit Tests (Jest)](/2d9118601eca80208d24fcfee60320a1)

## Example: Service With DB Dependency

### Production Code

```plain text
asyncfunctioncreateUser(userRepo, user) {
if (!user.email)thrownewError("Email required");
return userRepo.insert(user);
}
```

---

### Unit Test With Mock

```plain text
it("creates user successfully",async () => {
const repoMock = {
insert: jest.fn().mockResolvedValue({id:1 }),
  };

const result =awaitcreateUser(repoMock, {email:"a@test.com" });

expect(repoMock.insert).toHaveBeenCalledWith({email:"a@test.com" });
expect(result.id).toBe(1);
});
```

✔ DB not involved

✔ Logic isolated

---

# 🧱 Module Mocking (Common in Express Apps)

### Mocking a Module

```plain text
jest.mock("../database/mysql.database",() => ({
getPool: jest.fn(() => ({
execute: jest.fn(),
  })),
}));
```

Used when:

- Dependency is imported internally
- You cannot inject it easily

---

# ⏱ Mocking Time & Randomness

### Time

```plain text
jest.useFakeTimers();
jest.setSystemTime(newDate("2025-01-01"));
```

### Random

```plain text
jest.spyOn(Math,"random").mockReturnValue(0.5);
```

✔ Deterministic tests

---

# 🚦 Mocking Error Scenarios (Critical)

Mocking makes it easy to test failures.

```javascript
repoMock.insert.mockRejectedValue(newError("DB down"));

awaitexpect(createUser(repoMock, user)).rejects.toThrow("DB down");
```

✔ Without crashing real DB

✔ Controlled behavior

---

# 🧠 Mocking in TDD Context

In **TDD**, mocking:

- Emerges naturally
- Helps discover boundaries
- Improves design

> If you need too many mocks, your unit is doing too much.

---

# ⚖️ Mock vs Stub vs Spy (Interview Favorite)

| Feature | Stub | Mock | Spy |
| --- | --- | --- | --- |
| Returns data | ✔ | ✔ | ✔ |
| Verifies calls | ❌ | ✔ | ✔ |
| Runs real code | ❌ | ❌ | ✔ |

---

# 🧪 Best Practices for Mocking

## ✔ Mock at Boundaries

- DB
- External services

## ✔ Reset Mocks

```plain text
afterEach(() => {
  jest.clearAllMocks();
});
```

## ✔ Prefer Dependency Injection

```plain text
functionservice(repo) { ... }
```

Easier than module mocking.

---

## ✔ Assert Behavior, Not Calls (When Possible)

❌ Fragile

```javascript
expect(repo.insert).toHaveBeenCalledTimes(1);
```

✅ Better

```plain text
expect(result.id).toBeDefined();
```

---

# 🚫 Common Mocking Mistakes

| Mistake | Why It’s Bad |
| --- | --- |
| Mocking everything | Low confidence |
| Testing mock behavior | Testing fake code |
| Over-verifying calls | Brittle tests |
| Forgetting reset | Test leakage |
| Mocking internals | Breaks on refactor |

---

# 🏆 Benefits of Proper Mocking

✔ Fast tests

✔ True isolation

✔ Reliable failure simulation

✔ Better design

✔ Cleaner unit tests
