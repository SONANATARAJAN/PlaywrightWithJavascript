Here is your complete **Data-Driven Testing in JavaScript with Playwright** notes in `.md` format.

You can copy this into a file like:

```
data-driven-testing-playwright.md
```

---

# Data-Driven Testing in JavaScript with Playwright

---

# 1️⃣ Introduction to Data-Driven Testing (DDT)

## What is Data-Driven Testing?

**Data-Driven Testing (DDT)** is a test automation technique where:

* Test logic is written once
* Test data is stored separately
* Same test runs multiple times with different data inputs

Instead of writing multiple test cases manually, we loop through data.

---

## Why Use Data-Driven Testing?

✅ Avoid duplicate test code
✅ Improve maintainability
✅ Increase test coverage
✅ Separate logic and data
✅ Easy to scale

---

# 2️⃣ Basic Example Without Data-Driven Testing

```javascript
test('Login Test 1', async ({ page }) => {
  await page.goto('https://example.com/login');
  await page.fill('#username', 'user1');
  await page.fill('#password', 'pass1');
  await page.click('#login');
});

test('Login Test 2', async ({ page }) => {
  await page.goto('https://example.com/login');
  await page.fill('#username', 'user2');
  await page.fill('#password', 'pass2');
  await page.click('#login');
});
```

❌ Problem:

* Duplicate code
* Hard to maintain

---

# 3️⃣ Data-Driven Testing Using Array (Simple Method)

## Step 1: Create Test Data Array

```javascript
const loginData = [
  { username: 'user1', password: 'pass1' },
  { username: 'user2', password: 'pass2' },
  { username: 'user3', password: 'pass3' }
];
```

## Step 2: Loop Through Data

```javascript
import { test, expect } from '@playwright/test';

const loginData = [
  { username: 'user1', password: 'pass1' },
  { username: 'user2', password: 'pass2' }
];

loginData.forEach(data => {
  test(`Login Test for ${data.username}`, async ({ page }) => {
    await page.goto('https://example.com/login');
    await page.fill('#username', data.username);
    await page.fill('#password', data.password);
    await page.click('#login');

    await expect(page).toHaveURL(/dashboard/);
  });
});
```

✅ Clean
✅ Reusable
✅ Scalable

---

# 4️⃣ Data-Driven Testing Using JSON File

## Step 1: Create `loginData.json`

```json
[
  {
    "username": "user1",
    "password": "pass1"
  },
  {
    "username": "user2",
    "password": "pass2"
  }
]
```

## Step 2: Import JSON File

```javascript
import { test, expect } from '@playwright/test';
import loginData from './loginData.json';

loginData.forEach(data => {
  test(`Login Test for ${data.username}`, async ({ page }) => {
    await page.goto('https://example.com/login');
    await page.fill('#username', data.username);
    await page.fill('#password', data.password);
    await page.click('#login');

    await expect(page).toHaveURL(/dashboard/);
  });
});
```

---

# 5️⃣ Data-Driven Testing Using CSV File

## Step 1: Install CSV Parser

```bash
npm install csv-parser
```

## Step 2: Create `loginData.csv`

```
username,password
user1,pass1
user2,pass2
```

## Step 3: Read CSV in Playwright

```javascript
import fs from 'fs';
import csv from 'csv-parser';
import { test, expect } from '@playwright/test';

const results = [];

fs.createReadStream('loginData.csv')
  .pipe(csv())
  .on('data', (data) => results.push(data))
  .on('end', () => {
    results.forEach(data => {
      test(`Login Test for ${data.username}`, async ({ page }) => {
        await page.goto('https://example.com/login');
        await page.fill('#username', data.username);
        await page.fill('#password', data.password);
        await page.click('#login');
      });
    });
  });
```

---

# 6️⃣ Data-Driven Testing Using Excel File

## Step 1: Install XLSX Library

```bash
npm install xlsx
```

## Step 2: Read Excel File

```javascript
import XLSX from 'xlsx';
import { test, expect } from '@playwright/test';

const workbook = XLSX.readFile('loginData.xlsx');
const sheetName = workbook.SheetNames[0];
const sheet = workbook.Sheets[sheetName];
const data = XLSX.utils.sheet_to_json(sheet);

data.forEach(row => {
  test(`Login Test for ${row.username}`, async ({ page }) => {
    await page.goto('https://example.com/login');
    await page.fill('#username', row.username);
    await page.fill('#password', row.password);
    await page.click('#login');
  });
});
```

---

# 7️⃣ Using test.describe with Data

```javascript
test.describe('Login Tests', () => {

  const loginData = [
    { username: 'user1', password: 'pass1' },
    { username: 'user2', password: 'pass2' }
  ];

  for (const data of loginData) {
    test(`Login with ${data.username}`, async ({ page }) => {
      await page.goto('/login');
      await page.fill('#username', data.username);
      await page.fill('#password', data.password);
      await page.click('#login');
    });
  }

});
```

---

# 8️⃣ Positive & Negative Data-Driven Testing

```javascript
const loginData = [
  { username: 'validUser', password: 'validPass', expected: 'success' },
  { username: 'invalidUser', password: 'wrongPass', expected: 'error' }
];

loginData.forEach(data => {
  test(`Login Test: ${data.username}`, async ({ page }) => {
    await page.goto('/login');
    await page.fill('#username', data.username);
    await page.fill('#password', data.password);
    await page.click('#login');

    if (data.expected === 'success') {
      await expect(page).toHaveURL(/dashboard/);
    } else {
      await expect(page.locator('.error')).toBeVisible();
    }
  });
});
```

---

# 9️⃣ Advanced: Using Fixtures with Data

```javascript
test.describe('Data Driven with Fixtures', () => {

  const testData = require('./loginData.json');

  testData.forEach(data => {
    test(`Login: ${data.username}`, async ({ page }) => {
      await page.goto('/login');
      await page.fill('#username', data.username);
      await page.fill('#password', data.password);
      await page.click('#login');
    });
  });

});
```

---

# 🔟 Best Practices

✅ Keep test data separate from test logic
✅ Use JSON for medium complexity
✅ Use Excel for business-driven testing
✅ Use CSV for simple structured data
✅ Use environment variables for sensitive data
✅ Use unique test names

---

# 1️⃣1️⃣ Folder Structure Example

```
project/
│
├── tests/
│   ├── login.spec.js
│
├── testdata/
│   ├── loginData.json
│   ├── loginData.csv
│   ├── loginData.xlsx
│
├── playwright.config.js
```

---

# 1️⃣2️⃣ Common Mistakes

❌ Creating tests inside async callback (like CSV on 'end')
❌ Not generating unique test names
❌ Mixing test logic and data
❌ Hardcoding sensitive credentials

---

# 1️⃣3️⃣ Real-Time Example (Enterprise Pattern)

### loginData.js

```javascript
export const loginData = [
  {
    testCase: 'Valid Login',
    username: 'admin',
    password: 'admin123',
    expectedResult: 'success'
  },
  {
    testCase: 'Invalid Login',
    username: 'admin',
    password: 'wrong',
    expectedResult: 'failure'
  }
];
```

### login.spec.js

```javascript
import { test, expect } from '@playwright/test';
import { loginData } from '../testdata/loginData';

test.describe('Login Feature', () => {

  for (const data of loginData) {

    test(data.testCase, async ({ page }) => {

      await page.goto('/login');

      await page.fill('#username', data.username);
      await page.fill('#password', data.password);
      await page.click('#login');

      if (data.expectedResult === 'success') {
        await expect(page).toHaveURL(/dashboard/);
      } else {
        await expect(page.locator('.error')).toBeVisible();
      }

    });

  }

});
```

---

# 1️⃣4️⃣ When to Use Data-Driven Testing?

✔ Login Testing
✔ Form Validation
✔ API Testing
✔ Role-Based Testing
✔ Multiple Input Validation
✔ Boundary Value Testing

---

# 1️⃣5️⃣ Summary

| Method | Best For              |
| ------ | --------------------- |
| Array  | Small Data            |
| JSON   | Medium Data           |
| CSV    | Structured Data       |
| Excel  | Business Data         |
| DB     | Large Enterprise Data |

---

# 🎯 Final Conclusion

Data-Driven Testing in Playwright:

* Reduces code duplication
* Improves scalability
* Makes framework professional
* Helps handle large datasets
* Supports real-world enterprise automation

---

If you want, I can next provide:

* 🔥 Database-driven testing in Playwright
* 🔥 API Data-driven testing with Playwright
* 🔥 Page Object Model + Data Driven Integration
* 🔥 Complete Framework Structure
* 🔥 Interview Questions & Answers

Just tell me which one you need 🚀
