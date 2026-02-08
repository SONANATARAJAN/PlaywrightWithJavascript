# Playwright Test – Beginner Friendly Explanation (JavaScript)

This document explains the following Playwright test file **line by line**, covering **all basic concepts** required to understand and write Playwright tests confidently.

---

## 1. Sample Code Under Explanation

```javascript
const { test, expect } = require('@playwright/test')

test("My First Name", async function ({ page }) {
    expect(12).toBe(101)
})

test.skip("My Second test", async function ({ page }) {
    expect(12).toBe(12)
})

test("Third Test", async function ({ page }) {
    expect(12).toBe(12)
})

test("My Fourth test", async function ({ page }) {
    expect("Sona Natarajan").toContain("Sona")
    expect(true).toBeTruthy()
})

test("Fifth Test", async function ({ page }) {
    expect(false).toBeFalsy()
})
```

---

## 2. Importing Playwright Test Functions

```javascript
const { test, expect } = require('@playwright/test')
```

### Explanation:

* `@playwright/test` is Playwright’s **built-in test runner**
* `test` → used to define a test case
* `expect` → used for **assertions (validation)**

This replaces tools like **JUnit / TestNG / Jest**.

---

## 3. What is a Test Case?

```javascript
test("Test Name", async function ({ page }) {
   // test steps
})
```

### Key parts:

| Part           | Meaning                         |
| -------------- | ------------------------------- |
| `test()`       | Declares a test case            |
| First argument | Test name (shown in report)     |
| `async`        | Needed for browser actions      |
| `{ page }`     | Playwright browser page fixture |

---

## 4. Understanding the `page` Fixture

```javascript
async function ({ page })
```

### What is `page`?

* Represents a **single browser tab**
* Provided automatically by Playwright
* No need to create browser or driver manually

Example usage:

```javascript
await page.goto('https://example.com')
```

---

## 5. Assertions using `expect()`

Assertions verify whether the **actual result matches expected result**.

---

### 5.1 toBe() – Exact Match

```javascript
expect(12).toBe(101)
```

* Checks **strict equality (===)**
* This test ❌ FAILS because 12 ≠ 101

Use case:

* Numbers
* Booleans
* Exact string match

---

### 5.2 Skipping a Test

```javascript
test.skip("My Second test", async function ({ page }) {
    expect(12).toBe(12)
})
```

### Meaning:

* This test will **NOT execute**
* Shown as **SKIPPED** in report

### When to use:

* Feature under development
* Known bug
* Temporarily disabling tests

---

## 6. Passing Test Example

```javascript
expect(12).toBe(12)
```

* Actual = Expected
* Test ✅ PASSES

---

## 7. String Assertions

### 7.1 toContain()

```javascript
expect("Sona Natarajan").toContain("Sona")
```

* Checks **partial match** inside string
* Case-sensitive

✔ PASS because "Sona" exists in the string

---

## 8. Boolean Assertions

### 8.1 toBeTruthy()

```javascript
expect(true).toBeTruthy()
```

Passes if value is:

* `true`
* non-zero
* non-null

---

### 8.2 toBeFalsy()

```javascript
expect(false).toBeFalsy()
```

Passes if value is:

* `false`
* `0`
* `null`
* `undefined`

---

## 9. Test Execution Order

* Tests run **independently**
* Failure of one test does NOT stop others
* Order does not matter

---

## 10. Playwright Test Report

After execution:

```bash
npx playwright test
```

To view report:

```bash
npx playwright show-report
```

Report shows:

* Passed tests
* Failed tests
* Skipped tests
* Execution time

---

## 11. Common Assertion Methods (Basic Coverage)

| Assertion      | Purpose                   |
| -------------- | ------------------------- |
| `toBe()`       | Exact match               |
| `toContain()`  | Partial text              |
| `toBeTruthy()` | Truthy values             |
| `toBeFalsy()`  | Falsy values              |
| `toEqual()`    | Object / array comparison |
| `toHaveText()` | UI text validation        |

---

## 12. Why Async/Await is Mandatory

Playwright performs:

* Browser launch
* Page navigation
* Element interaction

These are **asynchronous operations**, so `async/await` is required.

---

## 13. Common Beginner Mistakes

❌ Forgetting `async`
❌ Missing `await`
❌ Using `expect` outside test block
❌ Hardcoding sleeps instead of auto-wait

---

## 14. Key Advantages Shown in This Example

* No WebDriver setup
* Built-in assertions
* Auto test isolation
* Simple syntax
* Clean readable tests

---

## 15. Summary

✔ `test()` defines a test case
✔ `expect()` validates results
✔ `test.skip()` disables tests
✔ Assertions decide PASS / FAIL
✔ Playwright handles browser automatically

---

**End of Playwright Basic Test Explanation**
