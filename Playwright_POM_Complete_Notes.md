Here is your complete **Playwright POM (Page Object Model) Notes** in **Markdown (.md) format**.
You can copy this into a file named:

```
Playwright_POM_Complete_Notes.md
```

---

# 📘 Playwright Page Object Model (POM) – Complete Notes

---

# 1️⃣ What is Page Object Model (POM)?

## 🔹 Definition

Page Object Model (POM) is a **design pattern** used in test automation where:

* Each web page is represented as a **separate class**
* Page elements (locators) are defined inside the class
* Page actions (methods) are also defined inside the class
* Test files only call the page methods

👉 This makes your framework:

* Clean
* Reusable
* Maintainable
* Scalable

---

# 2️⃣ Why Use POM?

Without POM ❌

* Locators inside test files
* Repeated code
* Hard to maintain
* If locator changes → update in many places

With POM ✅

* Locators in one file
* Reusable functions
* Easy maintenance
* Clean test structure

---

# 3️⃣ Folder Structure in Playwright (Recommended)

```
project/
│
├── pages/
│   ├── LoginPage.js
│   ├── DashboardPage.js
│
├── tests/
│   ├── login.spec.js
│
├── playwright.config.js
```

---

# 4️⃣ Basic POM Structure

## 📌 Step 1: Create Login Page Class

📁 `pages/LoginPage.js`

```javascript
class LoginPage {

    constructor(page) {
        this.page = page;
        this.username = '#username';
        this.password = '#password';
        this.loginButton = '#login';
        this.errorMessage = '.error-msg';
    }

    async enterUsername(username) {
        await this.page.fill(this.username, username);
    }

    async enterPassword(password) {
        await this.page.fill(this.password, password);
    }

    async clickLogin() {
        await this.page.click(this.loginButton);
    }

    async login(username, password) {
        await this.enterUsername(username);
        await this.enterPassword(password);
        await this.clickLogin();
    }

    async getErrorMessage() {
        return await this.page.textContent(this.errorMessage);
    }
}

module.exports = LoginPage;
```

---

# 5️⃣ Writing Test Using POM

📁 `tests/login.spec.js`

```javascript
const { test, expect } = require('@playwright/test');
const LoginPage = require('../pages/LoginPage');

test('Valid Login Test', async ({ page }) => {

    await page.goto('https://example.com/login');

    const loginPage = new LoginPage(page);

    await loginPage.login('student', 'Password123');

    await expect(page).toHaveURL('https://example.com/dashboard');
});
```

---

# 6️⃣ Important Concepts in POM

---

## 🔹 1. Constructor

```javascript
constructor(page) {
    this.page = page;
}
```

* `page` is Playwright’s page object
* Passed from test file
* Used to interact with browser

---

## 🔹 2. Locators Inside Class

```javascript
this.username = '#username';
```

Why?

* Centralized location
* Easy to update
* Avoid duplication

---

## 🔹 3. Action Methods

Instead of writing:

```javascript
await page.fill('#username', 'student');
```

We write:

```javascript
await loginPage.enterUsername('student');
```

This improves readability.

---

## 🔹 4. Reusable Combined Methods

```javascript
async login(username, password) {
    await this.enterUsername(username);
    await this.enterPassword(password);
    await this.clickLogin();
}
```

This is called:

> Business logic method

---

# 7️⃣ Advanced POM Example

## 📁 DashboardPage.js

```javascript
class DashboardPage {

    constructor(page) {
        this.page = page;
        this.profileIcon = '#profile';
        this.logoutButton = '#logout';
    }

    async clickProfile() {
        await this.page.click(this.profileIcon);
    }

    async logout() {
        await this.clickProfile();
        await this.page.click(this.logoutButton);
    }
}

module.exports = DashboardPage;
```

---

# 8️⃣ Using Multiple Page Objects

```javascript
const LoginPage = require('../pages/LoginPage');
const DashboardPage = require('../pages/DashboardPage');

test('Login and Logout Test', async ({ page }) => {

    const loginPage = new LoginPage(page);
    const dashboardPage = new DashboardPage(page);

    await page.goto('https://example.com/login');
    await loginPage.login('student', 'Password123');
    await dashboardPage.logout();
});
```

---

# 9️⃣ Best Practices in Playwright POM

✅ Keep locators private to class
✅ Use meaningful method names
✅ Avoid assertions inside page class (keep in test file)
✅ Keep test file clean
✅ Follow Single Responsibility Principle

---

# 🔟 POM With Playwright Locators (Recommended Modern Way)

Instead of:

```javascript
this.username = '#username';
```

Use Playwright locator:

```javascript
this.username = page.locator('#username');
```

Example:

```javascript
class LoginPage {

    constructor(page) {
        this.page = page;
        this.username = page.locator('#username');
        this.password = page.locator('#password');
        this.loginButton = page.locator('#login');
    }

    async login(username, password) {
        await this.username.fill(username);
        await this.password.fill(password);
        await this.loginButton.click();
    }
}
```

✅ More reliable
✅ Auto waiting feature
✅ Better performance

---

# 1️⃣1️⃣ POM With Fixtures (Advanced)

You can create custom fixtures in Playwright.

Example:

```javascript
const base = require('@playwright/test');

exports.test = base.test.extend({
    loginPage: async ({ page }, use) => {
        await use(new LoginPage(page));
    },
});
```

Now use in test:

```javascript
test('Login test', async ({ loginPage }) => {
    await loginPage.login('student', 'Password123');
});
```

---

# 1️⃣2️⃣ Advantages of POM

| Without POM       | With POM             |
| ----------------- | -------------------- |
| Repeated locators | Centralized locators |
| Hard to maintain  | Easy updates         |
| Messy test files  | Clean test structure |
| Poor reusability  | High reusability     |

---

# 1️⃣3️⃣ Common Mistakes

❌ Writing test logic inside page class
❌ Adding assertions inside POM
❌ Creating very large page classes
❌ Not separating pages properly

---

# 1️⃣4️⃣ Real-Time Framework Structure (Professional)

```
project/
│
├── pages/
│   ├── login.page.js
│   ├── dashboard.page.js
│
├── tests/
│   ├── login.spec.js
│
├── utils/
│   ├── testData.js
│
├── fixtures/
│   ├── baseFixture.js
```

---

# 1️⃣5️⃣ When To Use POM?

Use POM when:

* Project is medium or large
* Multiple test cases
* Multiple pages
* Team collaboration
* CI/CD integration

---

# 1️⃣6️⃣ Summary

Page Object Model in Playwright:

✔ Improves code structure
✔ Reduces duplication
✔ Increases reusability
✔ Easy maintenance
✔ Professional framework design

---

# 🎯 Final Interview Definition

> Page Object Model (POM) is a design pattern in automation where each webpage is represented as a class containing locators and reusable methods, which helps in improving maintainability, readability, and reusability of test scripts.

---

---

If you want next:

* 🔥 POM with TypeScript
* 🔥 POM with Data Driven Framework
* 🔥 Complete Real-Time Framework Setup
* 🔥 Playwright + POM + CI/CD
* 🔥 POM Interview Questions

Tell me 👍
