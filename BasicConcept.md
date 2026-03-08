# Playwright with JavaScript – Basic Concepts 

 
---

## 1. Installation & Setup

```bash
npm init -y
npm install -D @playwright/test
npx playwright install
```

```js
import { test, expect } from '@playwright/test';
```

---

## 2. Test Structure

```js
test('test name', async ({ page }) => {
  await page.goto('https://example.com');
});
```

---

## 3. Browser & Page

```js
const browser = await chromium.launch();
const context = await browser.newContext();
const page = await context.newPage();
```

---

## 4. Viewport

```js
await page.setViewportSize({ width: 1280, height: 720 });
```

```js
const context = await browser.newContext({
  viewport: { width: 1366, height: 768 }
});
```

---

## 5. Navigation

```js
await page.goto('https://example.com');
await page.reload();
await page.goBack();
await page.goForward();
```

---

## 6. Locators (Core Concept)

```js
const locator = page.locator('selector');
```

### Locator Types

```js
page.locator('css=button');
page.locator('xpath=//button');
page.getByText('Login');
page.getByRole('button', { name: 'Submit' });
page.getByLabel('Username');
page.getByPlaceholder('Enter name');
page.getByTestId('login-btn');
```

---

## 7. Locator Actions

```js
await locator.click();
await locator.fill('text');
await locator.type('text');
await locator.clear();
await locator.check();
await locator.uncheck();
await locator.selectOption('value');
await locator.hover();
```

---

## 8. Locator Assertions

```js
await expect(locator).toBeVisible();
await expect(locator).toBeHidden();
await expect(locator).toBeEnabled();
await expect(locator).toBeDisabled();
await expect(locator).toHaveText('Text');
await expect(locator).toContainText('Partial');
await expect(locator).toHaveValue('value');
```

---

## 9. Multiple Elements

```js
const items = page.locator('.item');
await items.nth(0).click();
await items.first().click();
await items.last().click();
```

---

## 10. Waiting (Auto & Manual)

```js
await page.waitForTimeout(3000); // delay
```

```js
await page.waitForSelector('#id');
await locator.waitFor();
```

---

## 11. Screenshots

```js
await page.screenshot({ path: 'page.png' });
```

```js
await locator.screenshot({ path: 'element.png' });
```

```js
await page.screenshot({ path: 'full.png', fullPage: true });
```

---

## 12. Keyboard & Mouse

```js
await page.keyboard.press('Enter');
await page.keyboard.type('Hello');
await page.mouse.click(100, 200);
```

---

## 13. Frames

```js
const frame = page.frame({ name: 'frameName' });
await frame.locator('button').click();
```

---

## 14. Alerts / Dialogs

```js
page.on('dialog', async dialog => {
  await dialog.accept();
});
```

---

## 15. Dropdowns

```js
await page.selectOption('#dropdown', 'value');
```

---

## 16. File Upload

```js
await page.setInputFiles('input[type=file]', 'file.txt');
```

---

## 17. Multiple Tabs / Windows

```js
const [newPage] = await Promise.all([
  context.waitForEvent('page'),
  page.click('#open')
]);
```

---

## 18. Authentication Storage

```js
await context.storageState({ path: 'auth.json' });
```

---

## 19. Test Hooks

```js
test.beforeEach(async ({ page }) => {});
test.afterEach(async ({ page }) => {});
```

---

## 20. Test Configuration (playwright.config.js)

```js
use: {
  headless: true,
  screenshot: 'only-on-failure',
  video: 'retain-on-failure'
}
```

---

## 21. Run Commands

```bash
npx playwright test
npx playwright test --headed
npx playwright test --ui
npx playwright show-report
```

---

## 22. Debugging

```bash
npx playwright test --debug
```

```js
await page.pause();
```

---

## 23. Environment Variables

```js
process.env.BASE_URL
```

---

## 24. API Testing (Basic)

```js
const response = await request.get('/users');
```

---

## 25. Retry & Timeout

```js
test.setTimeout(60000);
```

```js
retries: 2
```

---

**End of Notes**
