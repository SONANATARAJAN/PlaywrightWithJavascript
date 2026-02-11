 
````md
# 📌 Dropdown Handling in Playwright (JavaScript)

## 1️⃣ What is a Dropdown?

A **dropdown** is a UI element that allows users to select **one or more options** from a list.

### Common Types of Dropdowns:
1. **HTML `<select>` dropdown**
2. **Custom dropdowns** (div / ul / li based)
3. **Auto-suggestion dropdowns**
4. **Multi-select dropdowns**
5. **Searchable dropdowns**

---

## 2️⃣ Why Dropdown Handling is Important in Automation?

- Used in **forms**, **filters**, **settings**
- Common interview topic
- Frequently used in **real-time applications**
- Different dropdowns require **different handling approaches**

---

## 3️⃣ Playwright Overview for Dropdowns

Playwright provides:
- Built-in methods for `<select>` elements
- Powerful locators for custom dropdowns
- Auto-waiting and retry mechanism

---

## 4️⃣ Handling HTML `<select>` Dropdowns

### Example HTML
```html
<select id="country">
  <option value="IN">India</option>
  <option value="US">USA</option>
  <option value="UK">UK</option>
</select>
````

---

### ✅ Select by Value

```javascript
await page.selectOption('#country', 'IN');
```

---

### ✅ Select by Label (Visible Text)

```javascript
await page.selectOption('#country', { label: 'India' });
```

---

### ✅ Select by Index

```javascript
await page.selectOption('#country', { index: 1 });
```

---

### ✅ Select Multiple Options

```javascript
await page.selectOption('#country', ['IN', 'US']);
```

---

### ✅ Verify Selected Value

```javascript
const value = await page.locator('#country').inputValue();
expect(value).toBe('IN');
```

---

## 5️⃣ Handling Custom Dropdowns (Non-Select)

Custom dropdowns are usually built using:

* `<div>`
* `<ul><li>`
* JavaScript frameworks (React, Angular)

### Example HTML

```html
<div class="dropdown">
  <div class="selected">Select City</div>
  <ul class="options">
    <li>Chennai</li>
    <li>Bangalore</li>
    <li>Mumbai</li>
  </ul>
</div>
```

---

### ✅ Steps to Handle Custom Dropdown

1. Click dropdown
2. Locate option
3. Click required value

```javascript
await page.click('.selected');
await page.click('li:text("Chennai")');
```

---

## 6️⃣ Using `text=` Selector (Best Practice)

```javascript
await page.click('text=Chennai');
```

OR

```javascript
await page.locator('li', { hasText: 'Chennai' }).click();
```

---

## 7️⃣ Auto-Suggestion Dropdowns

Example:

* Google search suggestions
* Search-based dropdowns

### Example Flow

1. Type text
2. Wait for suggestions
3. Select matching value

```javascript
await page.fill('#search', 'playwright');

await page.waitForSelector('.suggestions li');

const options = await page.locator('.suggestions li');
await options.filter({ hasText: 'playwright tutorial' }).click();
```

---

## 8️⃣ Dropdown with Dynamic Data

If dropdown loads values dynamically:

```javascript
await page.click('#dropdown');
await page.waitForLoadState('networkidle');
await page.click('text=Option 1');
```

---

## 9️⃣ Loop Through Dropdown Options

### For `<select>` dropdown

```javascript
const options = await page.locator('#country option').allTextContents();
console.log(options);
```

---

### For Custom Dropdown

```javascript
const items = await page.locator('.options li');

for (let i = 0; i < await items.count(); i++) {
  console.log(await items.nth(i).innerText());
}
```

---

## 🔟 Validate Dropdown Values

```javascript
expect(await page.locator('#country option')).toHaveCount(3);
```

---

## 1️⃣1️⃣ Dropdown Inside iFrame

```javascript
const frame = page.frameLocator('#iframeId');
await frame.locator('#country').selectOption('IN');
```

---

## 1️⃣2️⃣ Common Mistakes

❌ Using `selectOption()` on non-`<select>` dropdown
❌ Not waiting for dropdown options
❌ Using absolute XPath
❌ Clicking without visibility check

---

## 1️⃣3️⃣ Best Practices

✅ Prefer `text=` locators
✅ Avoid hard waits (`waitForTimeout`)
✅ Use `hasText` filtering
✅ Validate selection after choosing
✅ Handle dynamic loading properly

---

## 1️⃣4️⃣ Interview Questions (Important)

### Q1: How do you handle dropdowns in Playwright?

* Only `<select>` → `selectOption()`
* Custom → click + locate option

### Q2: Can Playwright handle multi-select dropdowns?

✅ Yes, using array in `selectOption()`

### Q3: How to handle dynamic dropdowns?

* Wait for element
* Use `networkidle`
* Use visible text locator

---

## 1️⃣5️⃣ Summary

| Dropdown Type | Method               |
| ------------- | -------------------- |
| `<select>`    | `selectOption()`     |
| Custom        | Click + Text Locator |
| Auto-suggest  | Fill + Filter        |
| Multi-select  | Array selection      |
| iFrame        | `frameLocator()`     |

---

## ✅ Final Notes

* Playwright handles dropdowns **better than Selenium**
* Auto-waiting reduces flaky tests
* Text-based locators are most reliable


 
