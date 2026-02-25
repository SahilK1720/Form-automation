# Form Automation

## Overview

**Form Automation** is an automated script built using **Node.js and Playwright** that submits the daily worklog on the Kalvium internships portal automatically. It is built to ensure you never forget to fill the form and lose your marks (CGPA). Please use this as a backup and add your own static message.

The automation:

* Logs into Kalvium using a saved authenticated session
* Opens the daily worklog form
* Selects the required dropdown option
* Fills a predefined worklog message
* Submits the form
* Runs automatically on a schedule using **GitHub Actions**
* Sends an **email notification if the automation fails**

This project eliminates the need for manual daily submissions.

---

## Features

* Automated browser interaction using Playwright
* Headless execution on GitHub Actions (no laptop required)
* Uses saved authentication state (no repeated login)
* Scheduled execution (Mon–Fri at a fixed time)
* Failure email notification via Gmail SMTP
* Safe handling when the worklog is already submitted

---

## Tech Stack

* **Node.js**
* **Playwright**
* **GitHub Actions**
* **Gmail SMTP (App Password)**

---

## Project Structure

```
kalvium-daily-automation/
│
├── index.js              # Main automation script
├── package.json          # Project dependencies and scripts
├── package-lock.json
├── auth.json             # Login session (used locally, not committed)
├── .github/
│   └── workflows/
│       └── run.yml       # GitHub Actions workflow
└── README.md
```

---

## Prerequisites

### Local Machine

* Node.js (v18 or above)
* Google Chrome / Chromium
* Git

### GitHub

* GitHub repository
* GitHub Actions enabled
* GitHub Secrets configured

---

## Installation (Local Setup)

1. Clone the repository:

```bash
git clone https://github.com/<your-username>/kalvium-daily-automation.git
cd kalvium-daily-automation
```

2. Install dependencies:

```bash
npm install
```

3. Install Playwright browser:

```bash
npx playwright install chromium
```

---

## Authentication Setup

### Step 1: Generate auth.json (One-Time)

Run a temporary script to log in manually and save session:

```bash
node login.js
```

* Log in using **Google**
* Select your Kalvium email
* After successful login, `auth.json` will be created

⚠️ Do NOT commit `auth.json` to GitHub.

---

### Step 2: Local Environment Variable

For local testing, set the auth state:

**PowerShell**

```powershell
$env:AUTH_STATE = Get-Content auth.json -Raw
node index.js
```

---

## Running Locally

```bash
node index.js
```

Expected behavior:

* Opens Kalvium internships page
* Submits worklog if not already submitted
* Exits safely if already submitted

---

## GitHub Actions Setup

### Secrets Required

Add the following secrets in:
**GitHub → Repository → Settings → Secrets and Variables → Actions**

| Secret Name    | Description                     |
| -------------- | ------------------------------- |
| AUTH_STATE     | Content of `auth.json`          |
| EMAIL_USERNAME | Gmail address                   |
| EMAIL_PASSWORD | Gmail App Password              |
| EMAIL_TO       | Email to receive failure alerts |

---

### Scheduled Execution

The automation runs automatically using GitHub Actions:

* **Days:** Monday to Friday
* **Time:** 7:30 PM IST
* **Cron:** `0 14 * * 1-5` (UTC)

Workflow file:

```
.github/workflows/run.yml
```

You can also trigger it manually from the **Actions** tab.

---

## Failure Handling

* If the worklog is already submitted → script exits successfully
* If any error occurs (UI change, login issue, missing button):

  * GitHub Action fails
  * Failure email is sent automatically

---

## Important Notes

* This project uses **browser automation** and depends on UI structure
* If Kalvium changes the UI, selectors may need updates
* Authentication may expire and require regeneration of `auth.json`

---

## Disclaimer

This automation is created **for personal productivity and learning purposes only**.
---

## Author

**Sahil Vithal Kharatmol**

---

