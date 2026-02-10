# Playwright Codegen 

> **Purpose:** Auto-generate Playwright test code by recording browser actions

---

## 1. What is Codegen

* Built-in Playwright recorder
* Generates Playwright test code automatically
* Produces locators + actions
* Supports JavaScript / TypeScript

---

## 2. Basic Codegen Command

```bash
npx playwright codegen
```

---

## 3. Codegen with URL

```bash
npx playwright codegen https://example.com
```

---

## 4. Codegen with Specific Browser

```bash
npx playwright codegen --browser=chromium
npx playwright codegen --browser=firefox
npx playwright codegen --browser=webkit
```

---

## 5. Codegen Output Language

```bash
npx playwright codegen --target=javascript
npx playwright codegen --target=typescript
```

---

## 6. Codegen with Device Emulation

```bash
npx playwright codegen --device="iPhone 14"
npx playwright codegen --device="Pixel 7"
```

---

## 7. Codegen with Viewport

```bash
npx playwright codegen --viewport-size=1280,720
```

---

## 8. Codegen with Storage (Auth)

```bash
npx playwright codegen --load-storage=auth.json
```

---

## 9. Codegen with Output File

```bash
npx playwright codegen --output=login.spec.js
```

---

## 10. Codegen + Test Directory

```bash
npx playwright codegen --output=tests/login.spec.js
```

---

## 11. Codegen UI Panels

* Browser Window (record actions)
* Inspector Panel (generated code)
* Locator Picker (hover + select elements)

---

## 12. Codegen Generated Test Structure

```js
import { test, expect } from '@playwright/test';

test('test', async ({ page }) => {
  await page.goto('https://example.com');
});
```

---

## 13. Generated Locator Styles

```js
page.getByRole('button', { name: 'Login' });
page.getByText('Submit');
page.getByLabel('Username');
page.getByPlaceholder('Enter email');
page.getByTestId('login-btn');
page.locator('#id');
page.locator('.class');
page.locator('xpath=//div');
```

---

## 14. Generated Actions

```js
await page.click('selector');
await page.fill('selector', 'text');
await page.type('selector', 'text');
await page.check('selector');
await page.selectOption('selector', 'value');
await page.hover('selector');
```

---

## 15. Generated Assertions

```js
await expect(page.getByText('Text')).toBeVisible();
await expect(page.locator('#id')).toHaveText('value');
```

---

## 16. Codegen with Multiple Pages

```js
const page1 = await context.newPage();
const page2 = await context.newPage();
```

---

## 17. Codegen for New Tab / Popup

```js
const [page1] = await Promise.all([
  context.waitForEvent('page'),
  page.click('#open')
]);
```

---

## 18. Codegen + Frames

```js
const frame = page.frameLocator('#frame');
await frame.locator('button').click();
```

---

## 19. Codegen + Dialogs

```js
page.on('dialog', async dialog => {
  await dialog.accept();
});
```

---

## 20. Codegen + File Upload

```js
await page.setInputFiles('input[type=file]', 'file.txt');
```

---

## 21. Codegen + Keyboard

```js
await page.keyboard.press('Enter');
await page.keyboard.type('Hello');
```

---

## 22. Codegen + Mouse

```js
await page.mouse.move(100, 200);
await page.mouse.click(100, 200);
```

---

## 23. Codegen + Screenshot

```js
await page.screenshot({ path: 'page.png' });
```

---

## 24. Codegen + Full Page Screenshot

```js
await page.screenshot({ path: 'full.png', fullPage: true });
```

---

## 25. Codegen + Video / Trace (Config-based)

```js
use: {
  trace: 'on',
  video: 'on'
}
```

---

## 26. Codegen Debug Mode

```bash
npx playwright codegen --debug
```

---

## 27. Codegen Best Practices (Notes)

* Prefer `getByRole`, `getByLabel`
* Replace brittle CSS/XPath if needed
* Refactor generated code into POM
* Remove unnecessary waits

---

## 28. Convert Codegen to Page Object Model

```js
class LoginPage {
  constructor(page) {
    this.page = page;
    this.username = page.getByLabel('Username');
    this.password = page.getByLabel('Password');
    this.loginBtn = page.getByRole('button', { name: 'Login' });
  }
}
```

---

## 29. Codegen vs Manual Script

* Fast script creation
* Auto locator suggestions
* Good for beginners & demos

---

## 30. Exit Codegen

* Close browser window
* Stop terminal process (Ctrl + C)

---

**End of Playwright Codegen N
