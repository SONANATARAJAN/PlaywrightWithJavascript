
````md
# Playwright Iframe & Dynamic Network / Loader State – Complete Guide

This document covers:
- Iframe handling (static & dynamic)
- Network handling (API calls, waits)
- Loader / spinner handling
- Load states and best practices

All examples are written using **:contentReference[oaicite:0]{index=0}**.

---

## PART 1: IFRAME HANDLING IN PLAYWRIGHT

---

## 1. What is an Iframe?

An **iframe (Inline Frame)** is an HTML element that loads **another webpage inside the current page**.

```html
<iframe src="https://example.com"></iframe>
````

⚠️ Elements inside an iframe **cannot be accessed directly** using normal locators.

---

## 2. Why Iframes Are Important in Automation?

* Login widgets
* Payment gateways
* Ads
* Embedded dashboards

---

## 3. How Playwright Handles Iframes

Playwright provides **three ways**:

1. `frameLocator()` (RECOMMENDED)
2. `page.frame()`
3. `page.frames()`

---

## 4. Best Practice: `frameLocator()` ✅

### Example HTML

```html
<iframe id="loginFrame"></iframe>
```

### Playwright Code

```js
const frame = page.frameLocator('#loginFrame');

await frame.locator('#username').fill('admin');
await frame.locator('#password').fill('password');
await frame.locator('#login').click();
```

✔ Auto-wait
✔ Cleaner syntax
✔ Recommended by Playwright

---

## 5. Handling Iframe by Name or URL

```js
const frame = page.frame({ name: 'loginFrame' });
```

or

```js
const frame = page.frame({ url: /login/ });
```

---

## 6. Handling Multiple Iframes

```js
const frames = page.frames();

for (const frame of frames) {
  console.log(frame.url());
}
```

---

## 7. Nested Iframes (Iframe inside Iframe)

```js
const outerFrame = page.frameLocator('#outerFrame');
const innerFrame = outerFrame.frameLocator('#innerFrame');

await innerFrame.locator('#submit').click();
```

---

## 8. Common Iframe Mistakes ❌

❌ Trying to access iframe element directly

```js
await page.locator('#username').fill('admin'); // WRONG
```

❌ Using hard waits

```js
await page.waitForTimeout(5000);
```

---

## 9. Iframe Best Practices ✅

✔ Always use `frameLocator()`
✔ Avoid `waitForTimeout()`
✔ Verify iframe visibility
✔ Handle dynamic iframe loading

---

---

## PART 2: DYNAMIC NETWORK & LOADER STATE HANDLING

---

## 10. What is a Loader / Spinner?

A **loader** appears when:

* API call is in progress
* Page is loading data
* Backend response is pending

Examples:

* Spinners
* Progress bars
* Skeleton loaders

---

## 11. Load States in Playwright

| Load State         | Meaning              |
| ------------------ | -------------------- |
| `load`             | Page loaded          |
| `domcontentloaded` | DOM ready            |
| `networkidle`      | No network for 500ms |

---

## 12. Waiting for Page Load State

```js
await page.waitForLoadState('networkidle');
```

✔ Best for SPA applications

---

## 13. Handling Loader (Spinner) Disappearance

### HTML Loader Example

```html
<div id="loader">Loading...</div>
```

### Playwright Code

```js
await page.locator('#loader').waitFor({ state: 'hidden' });
```

✔ Most reliable way

---

## 14. Dynamic API Call Handling (IMPORTANT)

### Wait for Specific API Response

```js
await page.waitForResponse(response =>
  response.url().includes('/api/users') &&
  response.status() === 200
);
```

---

## 15. Trigger Action + Wait for API (Best Practice)

```js
await Promise.all([
  page.waitForResponse(resp =>
    resp.url().includes('/api/data') && resp.status() === 200
  ),
  page.click('#loadData')
]);
```

✔ Prevents race conditions
✔ Stable execution

---

## 16. Handling Multiple Network Calls

```js
await page.waitForLoadState('networkidle');
```

✔ Use when multiple APIs are fired

---

## 17. Intercepting Network Requests

```js
page.on('request', request => {
  console.log('Request:', request.url());
});

page.on('response', response => {
  console.log('Response:', response.url(), response.status());
});
```

---

## 18. Mocking API Response (Advanced)

```js
await page.route('**/api/users', route => {
  route.fulfill({
    status: 200,
    body: JSON.stringify({ name: 'Mock User' })
  });
});
```

✔ Useful for offline testing
✔ Faster execution

---

## 19. Real-Time Example (Iframe + Loader + API)

```js
test('iframe with loader and network wait', async ({ page }) => {
  await page.goto('https://example.com');

  const frame = page.frameLocator('#dataFrame');

  await Promise.all([
    page.waitForResponse(resp =>
      resp.url().includes('/api/details') && resp.status() === 200
    ),
    frame.locator('#fetchData').click()
  ]);

  await frame.locator('#loader').waitFor({ state: 'hidden' });

  await expect(frame.locator('#result')).toBeVisible();
});
```

---

## 20. Common Network Handling Mistakes ❌

❌ Using fixed timeout

```js
await page.waitForTimeout(10000);
```

❌ Not waiting for API

```js
await page.click('#submit');
```

---

## 21. Best Practices Summary ✅

✔ Use `frameLocator()` for iframes
✔ Use `networkidle` for SPA apps
✔ Wait for specific API responses
✔ Handle loader disappearance
✔ Avoid hard waits

---

## 22. Interview Questions & Answers

### Q1: How do you handle iframe in Playwright?

**Answer:** Using `frameLocator()` or `page.frame()`.

### Q2: Best way to wait for API response?

**Answer:** `page.waitForResponse()` with URL & status check.

### Q3: Difference between `load` and `networkidle`?

**Answer:** `load` waits for page load, `networkidle` waits for network silence.

---

## 23. Quick Cheat Sheet

```js
// Iframe
page.frameLocator('#frame')

// Loader
locator.waitFor({ state: 'hidden' })

// API wait
page.waitForResponse()

// Load state
page.waitForLoadState('networkidle')
```

---

## 24. Final Summary

* Iframes require special handling
* Network-based waits are more reliable
* Loader handling prevents flaky tests
* Event-driven waits > timeouts

---

📘 **You are now production-ready with Playwright iframe & network handling!**

```

---

### Want next?
✅ Shadow DOM  
✅ File upload & download  
✅ Playwright debugging strategies  
✅ Real interview failure scenarios with fixes  

Just tell me 👍
```
