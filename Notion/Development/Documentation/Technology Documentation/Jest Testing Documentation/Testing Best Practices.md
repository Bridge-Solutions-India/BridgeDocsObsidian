---

---

## 🔴🟢🔁 TDD Core Cycle (Always Follow This) 

TDD is the approach we gonna follow in our Backend projects

### **1. 🔴 RED – Write a Failing Test**

- Write a test **before** writing production code
- Test should describe **desired behavior**
- Test must **fail for the right reason**

✔ No implementation yet

✔ Clear failure reason

### **2. 🟢 GREEN – Write Minimum Code to Pass**

- Write the **simplest** code to make the test pass
- No refactoring
- No extra logic

✔ Ugly is fine

✔ Correct > Clean

### **3. 🔁 REFACTOR – Improve Without Breaking Tests**

- Clean up code
- Improve naming
- Remove duplication
- Optimize structure

✔ Tests must still pass

✔ No new behavior added

## 🧭 TDD DEVELOPMENT STEPS (REAL PROJECT FLOW)

### Step 1: Start From Behavior, Not Implementation

Ask:

> “What should the system do?”

**Example**

- ❌ “Create a MySQL insert function”
- ✅ “Reject request if body is invalid”

---

### Step 2: Write One Small Test at a Time

Avoid writing many tests at once.

❌ Bad

```javascript
it("should validate email, phone, name, description")
```

✅ Good

```javascript
it("should fail when email is missing")
it("should fail when email is invalid")
```

**Rule**

> One failing test → one small code change

---

## Step 3: Start With the Simplest Case

Always start with the **happy path**, then move to failures.

**Recommended order**

1. Valid input → success
2. Missing required fields
3. Invalid formats
4. Edge cases
5. System failures (DB down)

---

## Step 4: Test Public Interfaces Only

In TDD:

- Test **controllers, services, APIs**
- NOT private/helper methods

❌ Bad

```javascript
test("validateEmail() returns false")
```

✅ Good

```javascript
test("returns 400 when email is invalid")
```

**Why**

- Internal structure will change
- Behavior should not

---

## Step 5: Let Tests Drive Your Design

If a test feels **hard to write**, your design is probably wrong.

### Smell → Fix

| Smell | What it Means |
| --- | --- |
| Too many mocks | Too much responsibility |
| Huge setup | Poor separation |
| Complex assertions | Method doing too much |

✔ TDD naturally enforces **SRP (Single Responsibility Principle)**

---

## Step 6: Mock Only External Boundaries

In TDD:

- Start without mocks
- Introduce mocks **only when needed**

### What to Mock

✔ DB

✔ External APIs

✔ File systems

✔ Email/SMS

### What NOT to Mock

❌ Business logic

❌ Validation rules

❌ Domain models

```javascript
jest.mock("../database/mysql.database");
```

---

## Step 7: Keep Tests Fast (Non-Negotiable)

- Unit tests must run in **milliseconds**
- Integration tests should be limited

✔ Run tests on every save

✔ Fail fast

> Slow tests kill TDD adoption

---

## Step 8: Use Clear Test Naming (Critical in TDD)

Tests become **living documentation**.

```javascript
describe("POST /contact",() => {
it("returns 201 when request body is valid")
it("returns 400 when body is missing")
it("returns 500 when database fails")
});
```

You should understand behavior **without reading code**.

---

## Step 9: Refactor Aggressively (Protected by Tests)

Once tests pass:

- Rename variables
- Extract functions
- Split files
- Change structure

✔ If tests are green → refactor confidently

✔ If tests break → behavior changed unintentionally

---

## Step 10: Never Skip RED Phase

**Biggest TDD mistake:** writing code first “just this once”.

❌ Write code → then tests

❌ Write tests after implementation

✔ Test must fail before implementation

✔ Otherwise → it’s not TDD