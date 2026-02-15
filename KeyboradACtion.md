Below is a **complete, end-to-end guide** on **Keyboard Actions in Playwright (JavaScript)** written **fully in Markdown (.md)** format.
You can **copy-paste this directly into a `.md` file** for your notes or documentation.

---

````md
# 🎹 Keyboard Actions in Playwright (JavaScript)

Keyboard actions in Playwright allow you to simulate real user keyboard interactions such as typing text, pressing keys, using shortcuts (Ctrl, Alt, Shift), navigating forms, and triggering events.

Playwright provides **low-level** and **high-level** keyboard APIs that work reliably across browsers.

---

## 📌 Why Keyboard Actions Are Important

Keyboard actions are used when:
- Filling input fields
- Submitting forms using Enter
- Using shortcuts (Ctrl+A, Ctrl+C, Ctrl+V)
- Navigating using Tab / Arrow keys
- Testing accessibility (keyboard-only navigation)
- Triggering JS events bound to key presses

---

## 🧠 Keyboard API Overview

Playwright exposes keyboard actions via:

```js
page.keyboard
````

Main methods:

* `type()`
* `press()`
* `down()`
* `up()`
* `insertText()`

---

## ⌨️ 1. `page.keyboard.type()`

### 👉 What it does

Types text **character by character** with optional delay.

### ✅ Syntax

```js
await page.keyboard.type(text, options);
```

### 🔹 Example: Type into an input field

```js
await page.locator("#username").click();
await page.keyboard.type("sona123");
```

### 🔹 With delay (realistic typing)

```js
await page.keyboard.type("playwright", { delay: 100 });
```

📌 **Use when** you want to simulate natural typing.

---

## ⌨️ 2. `page.keyboard.press()`

### 👉 What it does

Presses a **single key or key combination**.

### ✅ Syntax

```js
await page.keyboard.press(key);
```

---

### 🔹 Press Enter

```js
await page.keyboard.press("Enter");
```

### 🔹 Press Tab

```js
await page.keyboard.press("Tab");
```

### 🔹 Press Escape

```js
await page.keyboard.press("Escape");
```

---

## ⌨️ 3. Special Keys Supported

| Key Name   | Usage                            |
| ---------- | -------------------------------- |
| Enter      | `"Enter"`                        |
| Tab        | `"Tab"`                          |
| Escape     | `"Escape"`                       |
| Backspace  | `"Backspace"`                    |
| Delete     | `"Delete"`                       |
| ArrowUp    | `"ArrowUp"`                      |
| ArrowDown  | `"ArrowDown"`                    |
| ArrowLeft  | `"ArrowLeft"`                    |
| ArrowRight | `"ArrowRight"`                   |
| Shift      | `"Shift"`                        |
| Control    | `"Control"`                      |
| Alt        | `"Alt"`                          |
| Meta       | `"Meta"` (Windows / Command key) |

---

## ⌨️ 4. Keyboard Shortcuts (Ctrl, Shift, Alt)

### 🔹 Ctrl + A (Select All)

```js
await page.keyboard.press("Control+A");
```

### 🔹 Ctrl + C (Copy)

```js
await page.keyboard.press("Control+C");
```

### 🔹 Ctrl + V (Paste)

```js
await page.keyboard.press("Control+V");
```

### 🔹 Shift + Tab

```js
await page.keyboard.press("Shift+Tab");
```

📌 On macOS, use `Meta` instead of `Control`.

---

## ⌨️ 5. `keyboard.down()` and `keyboard.up()`

### 👉 What it does

Allows **holding a key down** and releasing later.

### 🔹 Example: Hold Shift and type

```js
await page.keyboard.down("Shift");
await page.keyboard.type("playwright");
await page.keyboard.up("Shift");
```

➡️ Output: `PLAYWRIGHT`

---

### 🔹 Example: Ctrl + Click simulation

```js
await page.keyboard.down("Control");
await page.locator("#link").click();
await page.keyboard.up("Control");
```

---

## ⌨️ 6. `keyboard.insertText()`

### 👉 What it does

Inserts text **without triggering key events**.

### 🔹 Example

```js
await page.locator("#input").click();
await page.keyboard.insertText("Hello World");
```

📌 **Use when**:

* You don’t need key events
* Faster input
* Avoid JS listeners on keystrokes

---

## ⌨️ 7. Clear Input Using Keyboard

### 🔹 Backspace method

```js
await page.locator("#input").click();
await page.keyboard.press("Control+A");
await page.keyboard.press("Backspace");
```

---

## ⌨️ 8. Form Navigation Using Keyboard

### 🔹 Navigate using Tab

```js
await page.keyboard.press("Tab");
await page.keyboard.press("Tab");
await page.keyboard.type("value");
```

---

## ⌨️ 9. Dropdown Selection Using Keyboard

```js
await page.locator("#country").click();
await page.keyboard.press("ArrowDown");
await page.keyboard.press("ArrowDown");
await page.keyboard.press("Enter");
```

---

## ⌨️ 10. File Upload Keyboard Interaction (OS dialog)

❌ Keyboard **cannot** control native OS dialogs
✅ Use `setInputFiles()` instead

```js
await page.setInputFiles("#fileUpload", "test.pdf");
```

---

## ⌨️ 11. Keyboard + Locator Combination (Best Practice)

```js
await page.locator("#search").fill("playwright");
await page.locator("#search").press("Enter");
```

✔ Recommended over `page.keyboard.press()`
✔ Scoped to element
✔ More reliable

---

## ⌨️ 12. Assertions After Keyboard Actions

```js
await page.keyboard.press("Enter");
await expect(page.locator("#success")).toBeVisible();
```

---

## 🚀 Best Practices

✅ Prefer `locator.press()` over `page.keyboard.press()`
✅ Always focus element before typing
✅ Use keyboard actions for accessibility testing
✅ Avoid OS-level dialogs
❌ Do not mix `$` and keyboard APIs

---

## ❌ Common Mistakes

❌ Typing without focusing input
❌ Using keyboard for file uploads
❌ Relying only on delays
❌ Hardcoding OS-specific keys

---

## 🧪 Complete Example Test

```js
test("Keyboard actions demo", async ({ page }) => {
  await page.goto("https://example.com/login");

  await page.locator("#username").click();
  await page.keyboard.type("admin");

  await page.keyboard.press("Tab");
  await page.keyboard.type("password123");

  await page.keyboard.press("Enter");

  await expect(page.locator("#dashboard")).toBeVisible();
});
```

---

## 📚 Summary

| Method         | Use Case              |
| -------------- | --------------------- |
| `type()`       | Natural typing        |
| `press()`      | Single key / shortcut |
| `down()`       | Hold key              |
| `up()`         | Release key           |
| `insertText()` | Fast input            |

---

## ✅ Final Tip

> If a user can do it with a keyboard, **Playwright can automate it**.

---

**Happy Testing 🎯**

```

---

If you want next:
- 🔹 **Mouse actions in Playwright**
- 🔹 **Accessibility testing using keyboard**
- 🔹 **Keyboard vs Mouse best practices**
- 🔹 **Interview questions on keyboard actions**

Just tell me 👍
```
