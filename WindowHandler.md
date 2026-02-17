Below is a **complete, structured Markdown (`.md`) document** on **Window Handling in Playwright (Multiple / Different Windows & Tabs)** with **clear explanations, best practices, and examples**.

You can **copy-paste and save this as `playwright-window-handling.md`**.

---

````md
# Playwright Window Handling – Complete Guide

## 1. What is Window Handling?

**Window handling** means working with:
- Multiple browser windows
- New tabs
- Popups opened by user actions
- Child windows opened from a parent page

In **:contentReference[oaicite:0]{index=0}**, each **window or tab is represented as a `Page` object**.

---

## 2. Important Concepts (Must Know)

| Concept | Meaning |
|------|--------|
| Browser | Top-level browser instance |
| BrowserContext | Isolated session (like Incognito) |
| Page | A single tab or window |
| Popup | A new page opened by another page |

👉 **Every new window = new `Page`**

---

## 3. Difference Between Window Handling (Selenium vs Playwright)

| Selenium | Playwright |
|--------|------------|
| Window handles (String IDs) | Page objects |
| `getWindowHandles()` | `page.waitForEvent('popup')` |
| Switch using handle | Use new `Page` directly |

✔️ **Playwright is simpler and cleaner**

---

## 4. When Does a New Window Open?

A new window/tab opens when:
- Clicking a link with `target="_blank"`
- Clicking a button that opens a popup
- JavaScript `window.open()`

Example:
```html
<a href="https://example.com" target="_blank">Open</a>
````

---

## 5. Basic Window Handling Flow (IMPORTANT)

### Correct Order ✅

1. **Start waiting for popup**
2. **Trigger action**
3. **Capture new page**
4. **Work on new page**

---

## 6. Handling a New Window (Recommended Way)

### Example

```js
const [newPage] = await Promise.all([
  page.waitForEvent('popup'),
  page.click('#openWindow')
]);

await newPage.waitForLoadState();
console.log(await newPage.title());
```

### Explanation

* `waitForEvent('popup')` → waits for new window
* `page.click()` → triggers new window
* `newPage` → handle for child window

---

## 7. Why `Promise.all()` is Mandatory?

❌ Wrong (causes error):

```js
const [newPage] = await Promise.all([
  page.click('#openWindow')
]);
```

✔️ Correct:

```js
const [newPage] = await Promise.all([
  page.waitForEvent('popup'),
  page.click('#openWindow')
]);
```

### Reason

* `page.click()` is **not iterable**
* Popup must be listened **before** clicking

---

## 8. Handling New Tab Opened by Link

```js
const [tab] = await Promise.all([
  page.waitForEvent('popup'),
  page.click('a[target="_blank"]')
]);

await tab.waitForLoadState();
```

---

## 9. Switching Between Parent & Child Windows

### Parent Page

```js
const parentPage = page;
```

### Child Page

```js
const [childPage] = await Promise.all([
  page.waitForEvent('popup'),
  page.click('#openChild')
]);
```

### Switch Back to Parent

```js
await parentPage.bringToFront();
```

---

## 10. Closing Child Window

```js
await childPage.close();
```

✔️ Parent window remains active

---

## 11. Handling Multiple Windows

```js
page.on('popup', async popup => {
  await popup.waitForLoadState();
  console.log(await popup.title());
});
```

✔️ Useful when multiple windows open dynamically

---

## 12. Access All Open Windows

```js
const pages = page.context().pages();

for (const p of pages) {
  console.log(await p.title());
}
```

---

## 13. Real-Time Example Test

```js
import { test, expect } from '@playwright/test';

test('Handle new window', async ({ page }) => {
  await page.goto('https://example.com');

  const [newPage] = await Promise.all([
    page.waitForEvent('popup'),
    page.click('#openWindow')
  ]);

  await newPage.waitForLoadState();
  expect(await newPage.title()).toContain('Example');
});
```

---

## 14. Handling Popup Opened Automatically

```js
page.on('popup', async popup => {
  await popup.waitForLoadState();
  console.log('Popup opened:', await popup.url());
});
```

---

## 15. Common Mistakes ❌

### ❌ Using window handles

```js
getWindowHandles(); // Selenium concept – not Playwright
```

### ❌ Register popup listener after click

```js
await page.click('#open');
const popup = await page.waitForEvent('popup'); // TOO LATE
```

---

## 16. Best Practices ✅

✔ Always use `Promise.all()`
✔ Always wait for `popup` event
✔ Store parent page reference
✔ Close child windows when done
✔ Avoid hard waits (`waitForTimeout`)

---

## 17. Interview Questions & Answers

### Q1: How does Playwright handle new windows?

**Answer:** Each window is a `Page` object handled using `popup` event.

### Q2: Why use `waitForEvent('popup')`?

**Answer:** To reliably capture new window before it opens.

### Q3: Difference between `page` and `popup`?

**Answer:** `page` is current window, `popup` is newly opened child window.

---

## 18. Quick Cheat Sheet

```js
// Wait for new window
page.waitForEvent('popup')

// Switch to window
const newPage = popup

// Close window
await newPage.close()

// Switch back
await page.bringToFront()
```

---

## 19. Summary

* Playwright does **not** use window handles
* Every window/tab = `Page`
* Use `popup` event
* `Promise.all()` avoids race conditions
* Window handling is easier than Selenium

---

## 20. Conclusion

Playwright provides **clean, event-driven window handling**.
Mastering popup handling ensures **stable automation and interview success**.

📘 **Happy Testing with Playwright!**

```

---

If you want next:
- ✅ **Iframe handling**
- ✅ **File upload & download**
- ✅ **Shadow DOM**
- ✅ **Playwright interview scenarios (real failures + fixes)**

Just tell me 👍
```
