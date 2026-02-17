Below is a **complete, well-structured Markdown (`.md`) document** covering **Alerts in Playwright**, written in **simple, clear English with best practices and examples**.
You can **copy-paste and save it as `playwright-alerts.md`**.

---

````md
# Playwright Alerts – Complete Guide

## 1. What is an Alert in Web Applications?

An **alert** is a browser pop-up dialog used to:
- Show messages
- Ask for confirmation
- Take user input

Alerts are **browser-controlled**, not part of the DOM, so Playwright handles them using **events**, not locators.

---

## 2. Types of Alerts in Browsers

There are **three main types** of JavaScript dialogs:

| Alert Type | Purpose | Buttons |
|-----------|--------|---------|
| Alert | Show information | OK |
| Confirm | Get confirmation | OK, Cancel |
| Prompt | Get user input | OK, Cancel |

---

## 3. How Playwright Handles Alerts

Playwright listens to the **`dialog` event** on the `page` object.

```js
page.on('dialog', dialog => {
  // handle alert here
});
````

### Why?

Alerts:

* Are not HTML elements
* Block the browser until handled
* Must be accepted or dismissed immediately

---

## 4. Handling Simple Alert (OK button)

### Example JavaScript Alert

```js
alert("Login Successful");
```

### Playwright Code

```js
page.on('dialog', async dialog => {
  console.log(dialog.message());
  await dialog.accept();
});
```

### Explanation

* `dialog.message()` → gets alert text
* `dialog.accept()` → clicks OK

---

## 5. Handling Confirm Alert (OK / Cancel)

### Example JavaScript Confirm

```js
confirm("Are you sure?");
```

### Accept (OK)

```js
page.on('dialog', async dialog => {
  await dialog.accept();
});
```

### Dismiss (Cancel)

```js
page.on('dialog', async dialog => {
  await dialog.dismiss();
});
```

---

## 6. Handling Prompt Alert (Input Box)

### Example JavaScript Prompt

```js
prompt("Enter your name");
```

### Provide Input & Accept

```js
page.on('dialog', async dialog => {
  await dialog.accept("Sona");
});
```

### Cancel Prompt

```js
page.on('dialog', async dialog => {
  await dialog.dismiss();
});
```

---

## 7. Best Practice: Use `waitForEvent`

Avoid missing alerts by **waiting explicitly**.

### Recommended Way

```js
const dialogPromise = page.waitForEvent('dialog');
await page.click('#showAlert');

const dialog = await dialogPromise;
console.log(dialog.message());
await dialog.accept();
```

### Why This Is Better

* Prevents race conditions
* Ensures dialog is captured
* Cleaner test execution

---

## 8. Verify Alert Text (Assertion)

```js
const dialog = await page.waitForEvent('dialog');
expect(dialog.message()).toBe("Are you sure?");
await dialog.accept();
```

---

## 9. Handling Multiple Alerts

```js
page.on('dialog', async dialog => {
  console.log(dialog.type(), dialog.message());
  await dialog.accept();
});
```

### `dialog.type()` Values

* `alert`
* `confirm`
* `prompt`
* `beforeunload`

---

## 10. Handling `beforeunload` Alert

This appears when leaving a page.

```js
page.on('dialog', async dialog => {
  if (dialog.type() === 'beforeunload') {
    await dialog.accept();
  }
});
```

---

## 11. Common Mistakes ❌

### ❌ Clicking alert using locator

```js
await page.click('text=OK'); // WRONG
```

### ❌ Registering dialog handler after action

```js
await page.click('#alertBtn');
page.on('dialog', ...); // Too late
```

---

## 12. Correct Order ✅

```js
page.on('dialog', async dialog => {
  await dialog.accept();
});

await page.click('#alertBtn');
```

---

## 13. Full Example Test

```js
import { test, expect } from '@playwright/test';

test('Handle alert in Playwright', async ({ page }) => {
  await page.goto('https://example.com');

  page.on('dialog', async dialog => {
    expect(dialog.message()).toContain('success');
    await dialog.accept();
  });

  await page.click('#showAlert');
});
```

---

## 14. Interview Questions & Answers

### Q1: Why can’t Playwright use locators for alerts?

**Answer:** Alerts are browser dialogs, not DOM elements.

### Q2: What happens if alert is not handled?

**Answer:** Test will hang or fail due to blocked browser execution.

### Q3: How do you pass input to a prompt?

**Answer:** `dialog.accept("value")`

---

## 15. Summary

* Alerts are handled using `page.on('dialog')`
* Always register handler **before** triggering alert
* Use `waitForEvent` for reliability
* Cannot use locators for alerts
* Supports alert, confirm, prompt, beforeunload

---

## 16. Quick Cheat Sheet

```js
// Accept alert
await dialog.accept();

// Dismiss alert
await dialog.dismiss();

// Accept with input
await dialog.accept("text");

// Get alert message
dialog.message();

// Get alert type
dialog.type();
```

---

## 17. Conclusion

Playwright provides **powerful and reliable alert handling** through events.
Following best practices ensures **stable, flake-free tests**.

---

📘 **Happy Testing with Playwright!**

```

---

If you want next:
- ✅ **Playwright iframe handling**
- ✅ **File upload & download**
- ✅ **New window / popup handling**
- ✅ **Real interview problems with alerts**

Just tell me 👍
```
