
## What Is Mocking?

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
