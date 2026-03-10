 

This document explains **everything about file upload in Playwright** and **how to choose the best locators** using Playwright JavaScript.

Framework: :contentReference[oaicite:0]{index=0}  
Language: JavaScript  
Audience: QA, SDET, Automation Engineers

---

## 1️⃣ What is File Upload in Automation?

File upload automation means:
- Selecting a file from your local system
- Attaching it to an `<input type="file">`
- Submitting the form
- Verifying upload success

👉 In Playwright, **no OS file dialog is used**.  
Instead, Playwright **injects the file directly** into the input element.

---

## 2️⃣ HTML File Upload Basics

### Typical HTML
```html
<input type="file" id="fileInput">
<button type="submit">Upload</button>
````

Playwright interacts **only with the `<input type="file">`**, not the OS dialog.

---

## 3️⃣ Core API for File Upload

### ✅ `setInputFiles()` (MAIN API)

```js
await page.setInputFiles('#fileInput', 'test-data/sample.png');
```

or

```js
await page.locator('#fileInput').setInputFiles('sample.png');
```

✔ Fast
✔ Reliable
✔ Cross-platform
✔ No dialogs involved

---

## 4️⃣ Uploading Different File Types

### 📄 Upload any file

```js
await page.setInputFiles('#fileInput', 'files/report.pdf');
```

### 🖼 Upload image

```js
await page.setInputFiles('#fileInput', 'images/photo.jpg');
```

### 📊 Upload CSV

```js
await page.setInputFiles('#fileInput', 'data/users.csv');
```

---

## 5️⃣ Upload Multiple Files

HTML:

```html
<input type="file" multiple id="files">
```

Playwright:

```js
await page.setInputFiles('#files', [
  'file1.txt',
  'file2.txt'
]);
```

---

## 6️⃣ Upload from Memory (No Physical File)

```js
await page.setInputFiles('#fileInput', {
  name: 'hello.txt',
  mimeType: 'text/plain',
  buffer: Buffer.from('Hello Playwright')
});
```

✔ Useful for API + UI hybrid tests

---

## 7️⃣ Clearing an Uploaded File

```js
await page.setInputFiles('#fileInput', []);
```

---

## 8️⃣ Handling Hidden File Inputs

Playwright **can upload even if input is hidden**:

```html
<input type="file" hidden id="fileInput">
```

```js
await page.setInputFiles('#fileInput', 'sample.png');
```

✔ No need to remove `display:none`

---

## 9️⃣ Full File Upload Test Example

```js
test('File upload test', async ({ page }) => {
  await page.goto('https://practice.expandtesting.com/upload');

  await page.setInputFiles('#fileInput', 'sample.png');

  await page.getByTestId('file-submit').click();

  await expect(page.getByText('File uploaded')).toBeVisible();
});
```

---

# 🔍 LOCATORS IN PLAYWRIGHT — COMPLETE GUIDE

## 10️⃣ What is a Locator?

A **locator** tells Playwright **how to find an element** on the page.

Playwright uses **strict mode**:

> A locator must match **exactly one element**

---

## 11️⃣ Locator Priority (BEST → WORST)

| Priority | Locator Type  | Stability |
| -------- | ------------- | --------- |
| ⭐⭐⭐⭐⭐    | `getByTestId` | Best      |
| ⭐⭐⭐⭐     | `getByRole`   | Very good |
| ⭐⭐⭐      | `getByLabel`  | Good      |
| ⭐⭐⭐      | `getByText`   | Medium    |
| ⭐⭐       | CSS / ID      | Medium    |
| ⭐        | XPath         | Bad       |

---

## 12️⃣ All Locator Types with Examples

---

### ✅ 1. `getByTestId()` — BEST PRACTICE

HTML:

```html
<button data-testid="file-submit">Upload</button>
```

Playwright:

```js
await page.getByTestId('file-submit').click();
```

✔ Stable
✔ Designed for testing
✔ Strict-mode safe

---

### ✅ 2. `getByRole()` — Accessibility-based

```js
await page.getByRole('button', { name: 'Upload' }).click();
```

✔ User-facing
✔ Accessibility-friendly

⚠️ Must be **unique**

---

### ✅ 3. `getByLabel()` — Forms

HTML:

```html
<label for="fileInput">Choose file</label>
<input id="fileInput">
```

```js
await page.getByLabel('Choose file').setInputFiles('a.png');
```

✔ Best for forms

---

### ✅ 4. `getByText()`

```js
await page.getByText('Upload', { exact: true }).click();
```

⚠️ Breaks if text changes

---

### ✅ 5. CSS Selector

```js
await page.locator('#fileSubmit').click();
```

✔ Fast
⚠️ Less readable

---

### ⚠️ 6. XPath (Avoid)

```js
await page.locator('//button[text()="Upload"]').click();
```

❌ Brittle
❌ Hard to maintain

---

## 13️⃣ Strict Mode Error (Common Mistake)

❌ BAD:

```js
page.getByRole('button').click();
```

✔ FIX:

```js
page.getByTestId('file-submit').click();
```

---

## 14️⃣ How to Choose the RIGHT Locator

### ✅ Decision Rule

1️⃣ **Has `data-testid`?** → Use it
2️⃣ **User-facing action?** → `getByRole()`
3️⃣ **Form field?** → `getByLabel()`
4️⃣ **Nothing else?** → CSS ID
5️⃣ **Never start with XPath**

---

## 15️⃣ Debugging Locators

```js
await page.pause();
```

```bash
npx playwright codegen
```

✔ Shows best locator automatically

---

## 16️⃣ File Upload Best Practices

✅ Use `setInputFiles()`
✅ Avoid OS dialogs
✅ Prefer `data-testid`
❌ Avoid `waitForTimeout()`
❌ Avoid XPath
❌ Avoid generic locators

---

## 17️⃣ Interview Tips

**Q: How does Playwright upload files without dialog?**
✔ By directly setting the file input value

**Q: Best locator?**
✔ `getByTestId()`

**Q: Why strict mode exists?**
✔ To prevent flaky tests

---

## ✅ Final Summary

✔ Playwright file upload is fast & reliable
✔ `setInputFiles()` is the only correct method
✔ Locator choice determines test stability
✔ Strict mode helps catch bad locators early

---

🎯 You now have **real-world Playwright knowledge**, not just syntax.

```

---

If you want next, I can:
- split this into **two separate `.md` files**
- add **real interview Q&A**
- add **Playwright file upload edge cases**
- convert this into **PDF / notes format**

Just tell me 👍
```
