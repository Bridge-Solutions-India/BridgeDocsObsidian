
## What Is Integration Testing?

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
