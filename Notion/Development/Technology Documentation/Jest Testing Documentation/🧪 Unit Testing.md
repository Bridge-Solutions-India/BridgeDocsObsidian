
## What Is Unit Testing?

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

