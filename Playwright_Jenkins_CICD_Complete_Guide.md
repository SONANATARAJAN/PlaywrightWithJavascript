 

 
---

# 1️⃣ What is CI/CD?

## 🔹 CI – Continuous Integration

Continuous Integration means:

* Developers push code to Git repository
* Automatically build + test runs
* Errors detected early

## 🔹 CD – Continuous Delivery / Deployment

Continuous Delivery means:

* After successful build & test
* Code is ready for deployment
* Can be automatically deployed

---

# 2️⃣ Why CI/CD for Playwright?

In automation:

Without CI ❌

* Run tests manually
* Time consuming
* Human error
* No build tracking

With CI/CD (Jenkins) ✅

* Tests run automatically
* Every code push is validated
* HTML reports generated
* Team gets email notification
* Integrated with GitHub / GitLab

---

# 3️⃣ What is Jenkins?

Jenkins is:

* Open-source automation server
* Used for CI/CD
* Runs builds automatically
* Integrates with Git

---

# 4️⃣ Architecture Overview

```
Developer → GitHub → Jenkins → Install Dependencies → Run Playwright Tests → Generate Report → Notify Team
```

---

# 5️⃣ Prerequisites

Install:

* Node.js
* Playwright
* Jenkins
* Git

---

# 6️⃣ Install Jenkins (Windows)

1. Download from official site
2. Install as Windows service
3. Open:

```
http://localhost:8080
```

4. Unlock Jenkins
5. Install suggested plugins

---

# 7️⃣ Sample Playwright Project Structure

```
project/
│
├── tests/
│   ├── login.spec.js
│
├── playwright.config.js
├── package.json
```

---

# 8️⃣ Sample package.json

```json
{
  "name": "playwright-project",
  "version": "1.0.0",
  "scripts": {
    "test": "npx playwright test",
    "report": "npx playwright show-report"
  },
  "devDependencies": {
    "@playwright/test": "^1.40.0"
  }
}
```

---

# 9️⃣ Important Playwright Commands

Install browsers:

```bash
npx playwright install
```

Run tests:

```bash
npx playwright test
```

Generate report:

```bash
npx playwright show-report
```

---

# 🔟 CI/CD Pipeline Flow in Jenkins

1. Pull code from Git
2. Install dependencies
3. Install Playwright browsers
4. Run tests
5. Publish report
6. Send notification

---

# 1️⃣1️⃣ Create Jenkins Freestyle Project

### Step 1: New Item

* Click "New Item"
* Select "Freestyle Project"

### Step 2: Source Code Management

* Select Git
* Add Repository URL
* Add credentials

### Step 3: Build Steps → Execute Windows Batch

Add:

```bash
npm install
npx playwright install
npx playwright test
```

Save → Build Now

---

# 1️⃣2️⃣ Using Jenkinsfile (Pipeline – Recommended)

Instead of Freestyle, use Pipeline.

Create file in project root:

```
Jenkinsfile
```

---

# 1️⃣3️⃣ Basic Jenkinsfile for Playwright

```groovy
pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                git 'https://github.com/your-repo/playwright-project.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                bat 'npm install'
            }
        }

        stage('Install Browsers') {
            steps {
                bat 'npx playwright install'
            }
        }

        stage('Run Tests') {
            steps {
                bat 'npx playwright test'
            }
        }

        stage('Publish Report') {
            steps {
                publishHTML([
                    reportDir: 'playwright-report',
                    reportFiles: 'index.html',
                    reportName: 'Playwright Report'
                ])
            }
        }
    }
}
```

---

# 1️⃣4️⃣ For Linux Jenkins Server

Replace:

```
bat
```

With:

```
sh
```

Example:

```groovy
sh 'npm install'
```

---

# 1️⃣5️⃣ Headless Mode in CI

Playwright runs in headless mode by default.

But ensure config:

```javascript
use: {
  headless: true
}
```

---

# 1️⃣6️⃣ Parallel Execution in CI

In `playwright.config.js`:

```javascript
workers: 4
```

This speeds up CI execution.

---

# 1️⃣7️⃣ Environment Variables in Jenkins

Example:

```groovy
environment {
    BASE_URL = "https://test-env.com"
}
```

Use in Playwright:

```javascript
process.env.BASE_URL
```

---

# 1️⃣8️⃣ Trigger Build Automatically

Go to:

Build Triggers →

✔ GitHub hook trigger
✔ Poll SCM

Now every push triggers Jenkins.

---

# 1️⃣9️⃣ Email Notification Setup

Install Email Extension Plugin.

Add post section:

```groovy
post {
    always {
        emailext (
            subject: "Build Status",
            body: "Check Jenkins for details",
            to: "team@example.com"
        )
    }
}
```

---

# 2️⃣0️⃣ Publish Playwright HTML Report

Install:

HTML Publisher Plugin

Add:

```groovy
publishHTML([
  allowMissing: false,
  alwaysLinkToLastBuild: true,
  keepAll: true,
  reportDir: 'playwright-report',
  reportFiles: 'index.html',
  reportName: 'Playwright HTML Report'
])
```

---

# 2️⃣1️⃣ Docker-Based CI/CD (Advanced)

You can run Playwright in Docker.

Create Dockerfile:

```dockerfile
FROM mcr.microsoft.com/playwright:v1.40.0-jammy

WORKDIR /app
COPY . .
RUN npm install

CMD ["npx", "playwright", "test"]
```

Jenkins runs inside Docker container.

---

# 2️⃣2️⃣ CI/CD Best Practices

✅ Run tests in headless mode
✅ Keep tests independent
✅ Use environment configs
✅ Store credentials securely
✅ Use pipeline instead of freestyle
✅ Enable parallel execution
✅ Archive reports

---

# 2️⃣3️⃣ Complete CI/CD Flow Diagram

```
Developer Push Code
        ↓
GitHub Repository
        ↓
Jenkins Trigger
        ↓
Install Dependencies
        ↓
Run Playwright Tests
        ↓
Generate HTML Report
        ↓
Email / Slack Notification
        ↓
Deploy (Optional)
```

---

# 2️⃣4️⃣ Common CI Issues

❌ Browsers not installed
Solution → `npx playwright install`

❌ Node version mismatch
Solution → Configure Node in Jenkins

❌ Permission denied
Solution → Fix workspace permission

❌ Report not visible
Solution → Install HTML Publisher plugin

---

# 2️⃣5️⃣ Real-Time Professional CI/CD Structure

```
project/
│
├── tests/
├── pages/
├── utils/
├── playwright.config.js
├── package.json
├── Jenkinsfile
├── Dockerfile
```

---

# 2️⃣6️⃣ Interview Questions

1. What is CI/CD?
2. Why Jenkins for Playwright?
3. Difference between Freestyle & Pipeline?
4. How to publish Playwright reports?
5. How to trigger Jenkins on Git push?
6. How to handle environment variables?
7. How to run tests in parallel?
8. How to run Playwright in Docker CI?

---

# 🎯 Final Interview Definition

> CI/CD in Playwright using Jenkins means automatically building, installing dependencies, executing Playwright tests, generating reports, and notifying stakeholders whenever code is pushed to the repository, ensuring continuous quality validation.

 
