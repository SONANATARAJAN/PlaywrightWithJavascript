# Node.js & Playwright Installation Notes

## 1. What is Node.js?

Node.js is a JavaScript runtime built on Chrome’s V8 engine. It allows you to run JavaScript outside the browser and is required to run Playwright tests using JavaScript/TypeScript.

---

## 2. Prerequisites

Before installing Playwright, ensure the following:

* Windows / Linux / macOS
* Internet access
* Admin or sudo access (recommended)

---

## 3. Install Node.js

### 3.1 Check if Node.js is already installed

```bash
node -v
npm -v
```

If versions are shown, Node.js is already installed.

---

### 3.2 Download Node.js

* Go to: [https://nodejs.org](https://nodejs.org)
* Download **LTS version** (recommended for Playwright)

---

### 3.3 Install Node.js (Windows)

1. Run the `.msi` installer
2. Click **Next → Next → Install**
3. Select **Add to PATH** (important)
4. Finish installation

Verify:

```bash
node -v
npm -v
```

---

### 3.4 Install Node.js (Linux)

```bash
sudo apt update
sudo apt install nodejs npm -y
```

Verify:

```bash
node -v
npm -v
```

---

## 4. Create Playwright Project

### 4.1 Create a new folder

```bash
mkdir playwright-project
cd playwright-project
```

---

### 4.2 Initialize Node project

```bash
npm init -y
```

This creates `package.json`.

---

## 5. Install Playwright

### 5.1 Install Playwright Test

```bash
npm init playwright@latest
```

---

### 5.2 Installation prompts explanation

You will be asked:

* ✔ Language: **JavaScript or TypeScript**
* ✔ Test folder: `tests`
* ✔ Add GitHub Actions: Optional
* ✔ Install Playwright browsers: **Yes (required)**

---

### 5.3 What gets installed

* Playwright Test framework
* Browsers:

  * Chromium
  * Firefox
  * WebKit
* Configuration files

---

## 6. Project Structure

```
playwright-project/
│
├── tests/
│   └── example.spec.js
├── playwright.config.js
├── package.json
└── node_modules/
```

---

## 7. Run Playwright Tests

### 7.1 Run all tests

```bash
npx playwright test
```

---

### 7.2 Run tests in headed mode

```bash
**npx playwright test --headed
**```

---

### 7.3 Run tests in specific browser

```bash
npx playwright test --project=chromium
```

---

## 8. Open Playwright Report

```bash
npx playwright show-report
```

---

## 9. Playwright Codegen (Record & Playback)

```bash
npx playwright codegen https://example.com
```

* Opens browser
* Records actions
* Generates Playwright code

---

## 10. Install Playwright Manually (Optional)

```bash
npm install -D @playwright/test
npx playwright install
```

---

## 11. Verify Playwright Installation

```bash
npx playwright --version
```

---

## 12. Common Issues & Fixes

### Issue: node command not found

* Restart terminal
* Check PATH variable

### Issue: Browser not found

```bash
npx playwright install
```

---

## 13. Why Playwright?

* Free & Open Source
* Cross-browser testing
* Auto-waiting
* Web-first assertions
* Shadow DOM support
* Fast execution
* Built-in tracing
* Codegen support

---

## 14. Useful Commands Summary

```bash
node -v
npm -v
npm init playwright@latest
npx playwright test
npx playwright show-report
npx playwright codegen
```

---

## 15. Conclusion

Node.js is mandatory for Playwright (JS/TS). After Node installation, Playwright setup is straightforward using `npm init playwright@latest`. This setup supports modern web automation with minimal configuration.

---

**End of Notes**
