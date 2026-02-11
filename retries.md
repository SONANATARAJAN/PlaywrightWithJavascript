# Playwright Retries – Complete Notes (.md)

> **Scope:** Test retries, assertion retries, action retries, config-level retries

---

## 1. What are Retries in Playwright

* Automatic re-run of failed tests
* Built-in retry mechanism
* Works at test, describe, project, and config levels

---

## 2. Enable Retries (Global – playwright.config.js)

```js
import { defineConfig } from '@playwright/test';

export default defineConfig({
  retries: 2
});
```

---

## 3. Retries per Project

```js
projects: [
  {
    name: 'Chromium',
    use: { browserName: 'chromium' },
    retries: 1
  },
  {
    name: 'Firefox',
    use: { browserName: 'firefox' },
    retries: 2
  }
]
```

---

## 4. Retries for Specific Test

```js
test('retry test', async ({ page }) => {
  test.retry(2);
});
```

---

## 5. Retries for Test Group (describe)

```js
test.describe('flaky tests', () => {
  test.retry(3);

  test('test 1', async ({ page }) => {});
  test('test 2', async ({ page }) => {});
});
```

---

## 6. Conditional Retries (CI Only)

```js
retries: process.env.CI ? 2 : 0
```

---

## 7. Retries with Command Line

```bash
npx playwright test --retries=2
```

---

## 8. Difference: Retries vs Re-run

* Retries → automatic re-execution
* Re-run → manual execution

---

## 9. Retry Count Behavior

* Initial run = attempt 0
* Retry 1 = attempt 1
* Retry 2 = attempt 2

---

## 10. Access Retry Information

```js
test('retry info', async ({}, testInfo) => {
  console.log(testInfo.retry);
});
```

---

## 11. Conditional Logic Based on Retry

```js
if (testInfo.retry > 0) {
  // retry-specific logic
}
```

---

## 12. Screenshots on Retry

```js
use: {
  screenshot: 'only-on-failure'
}
```

---

## 13. Video on Retry

```js
use: {
  video: 'retain-on-failure'
}
```

---

## 14. Trace on Retry (Best Practice)

```js
use: {
  trace: 'on-first-retry'
}
```

---

## 15. Assertion Retries (Auto-Wait)

```js
await expect(locator).toBeVisible();
await expect(locator).toHaveText('Text');
```

---

## 16. Action Retries (Auto-Retry)

```js
await page.click('#btn');
await locator.fill('value');
```

---

## 17. Difference: Test Retry vs Assertion Retry

* Test Retry → reruns entire test
* Assertion Retry → retries condition until timeout

---

## 18. Retry with Timeout

```js
expect(locator).toBeVisible({ timeout: 5000 });
```

---

## 19. Disabling Retries for Specific Test

```js
test('no retry test', async ({ page }) => {
  test.retry(0);
});
```

---

## 20. Retries with Serial Mode

```js
test.describe.configure({ mode: 'serial', retries: 2 });
```

---

## 21. Retries with Parallel Mode

```js
test.describe.configure({ mode: 'parallel', retries: 1 });
```

---

## 22. Retries and Test Status

* flaky → passes after retry
* failed → fails after all retries
* passed → no retry needed

---

## 23. Retry Reporting

* Retry count shown in HTML report
* Retry logs available per attempt

---

## 24. Best Practices for Retries

* Use retries only for flaky tests
* Fix root cause of failures
* Prefer assertion auto-wait
* Enable trace on first retry

---

## 25. Common Anti-Patterns

* High retry count
* Retrying broken tests
* Using retries instead of waits

---

## 26. Retry vs waitForTimeout

* Retries → test re-execution
* waitForTimeout → hard delay

---

## 27. Retry Example (End-to-End)

```js
test('login retry', async ({ page }) => {
  test.retry(2);
  await page.goto('/login');
  await page.fill('#user', 'admin');
  await page.fill('#pass', 'password');
  await page.click('#login');
  await expect(page.locator('h1')).toHaveText('Dashboard');
});
```

---

## 28. Retry with CI Stability Strategy

* retries: 2
* trace: on-first-retry
* screenshot: only-on-failure
* video: retain-on-failure

---

## 29. Interview Keywords (Retries)

* Flaky Tests
* Auto-Wait
* Test Stability
* Retry Mechanism
* CI Reliability

---

## 30. Summary

* Retries improve stability
* Use sparingly
* Always analyze flaky causes

---

**End of Playwright Retries Notes**
