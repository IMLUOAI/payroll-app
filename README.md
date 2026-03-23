# 💰 Payroll App — Double Sided ISCM LLC

A lightweight, mobile-first weekly payroll management application built as a single HTML file. Runs entirely in the browser with no backend required. Designed to be installed on an iPhone home screen and used daily for employee time tracking, wage calculation, and OneDrive Excel export.

---

## Table of Contents

- [Features](#features)
- [Live App](#live-app)
- [Getting Started — GitHub Pages](#getting-started--github-pages)
- [iPhone Installation](#iphone-installation)
- [Daily Usage](#daily-usage)
- [OneDrive & Azure Setup](#onedrive--azure-setup)
- [Technical Architecture](#technical-architecture)
- [Data & Privacy](#data--privacy)

---

## Features

### Time & Wage Tracking
- Clock-in and clock-out entry for each employee per day
- Break duration in minutes, automatically deducted before calculation
- Real-time net hours and pay calculation as data is entered
- Overtime calculated at **1.5×** for hours exceeding 8/day or 40/week
- Visual ⚠️ OT warning badge and amber card border when overtime is triggered

### Employee Management
- Add, rename, and remove employees at any time
- Editable name, role, and hourly rate per employee
- Color-coded employee cards for quick identification
- Week notes per employee for remarks, sick days, bonuses, or deductions

### Scheduling Tools
- **⚡ Shift Templates** — save common shifts (e.g. 9am–5pm, 30 min break) and apply in one tap
- **↩ Copy Last Week** — duplicate the previous week's schedule to the current week instantly
- Toggle individual days on/off per employee

### Navigation
- Week picker with full calendar view — tap any date to jump to that week
- Prev/Next week buttons and "This Week" shortcut
- **Swipe left/right** on mobile to navigate between weeks
- Monthly overview panel showing payroll totals for every week in the month

### Export & Reporting
- **📊 Save to Excel** — appends weekly data to a master Excel file in OneDrive via Microsoft Graph API
- **↑ Share CSV** — exports current week as CSV and routes through the iOS Share Sheet
- **↓ All Weeks CSV** — exports the full payroll history across all recorded weeks
- Print-friendly layout
- Weekly payroll summary with per-employee breakdown
- Live sticky bar showing running weekly total, net hours, OT hours, and staff count

### App Experience
- Installable on iPhone home screen (PWA-ready)
- Works fully offline after initial load
- Dark and light mode toggle, preference saved automatically
- Data persists across sessions via browser localStorage
- iOS safe area support (notch, home indicator)

---

## Live App

```
https://<your-github-username>.github.io/<repository-name>
```

Replace with your actual GitHub username and repository name after completing the setup below.

---

## Getting Started — GitHub Pages

### Step 1 — Create a repository

1. Sign in to [github.com](https://github.com)
2. Click **+** → **New repository**
3. Name it `payroll-app` (or any name you prefer)
4. Set visibility to **Public**
5. Check **Add a README file**
6. Click **Create repository**

### Step 2 — Upload the app file

1. In your new repository, click **Add file** → **Upload files**
2. Upload `weekly-wage-calculator.html` and this `README.md`
3. Click **Commit changes**

### Step 3 — Rename the HTML file to index.html

1. Click on `weekly-wage-calculator.html` in the repository
2. Click the **pencil (Edit) icon**
3. Change the filename at the top to `index.html`
4. Click **Commit changes**

### Step 4 — Enable GitHub Pages

1. Go to the **Settings** tab of your repository
2. Click **Pages** in the left sidebar
3. Under **Branch**, select `main` and leave the folder as `/ (root)`
4. Click **Save**

Your app will be live at your GitHub Pages URL within 1–2 minutes.

---

## iPhone Installation

The app must be opened in **Safari** for home screen installation to work. Chrome and other browsers do not support this feature on iOS.

1. Open Safari on your iPhone and navigate to your GitHub Pages URL
2. Tap the **Share button** (box with arrow pointing up ↑) at the bottom of Safari
3. Scroll down and tap **"Add to Home Screen"**
4. Name it **Payroll** and tap **Add**

The app will appear on your home screen with a gold **$** icon and will launch in fullscreen, behaving like a native application.

---

## Daily Usage

### Entering hours

1. Open the app from your home screen
2. Locate the employee card
3. Tap the **toggle switch** on the right side of the day's row to activate that day
4. Tap the **In** field and set the clock-in time
5. Tap the **Out** field and set the clock-out time
6. Enter break minutes in the **Brk** field (e.g. `30` for a 30-minute break)
7. Net hours and pay update instantly — no button to press

### Using shift templates

1. Tap **⚡ Tpl** on any employee card footer to open the templates panel
2. Select a saved template to apply it to a specific day, or tap the **⚡ icon** next to the day toggle
3. To save a new template, enter a name, clock-in, clock-out, and break minutes, then tap **Save Template**

### Copying the previous week

Tap **↩ Copy** on any employee card to copy that employee's entire previous week schedule into the current week. A confirmation prompt will appear before overwriting any existing entries.

### Saving to OneDrive

After entering the week's data, tap **📊 Save to Excel** in the Weekly Payroll Summary section. The app will append the current week's rows to the master Excel file at `/Double Sided ISCM/Payroll/Payroll_Master.xlsx` on your OneDrive. Existing records are never overwritten.

### Navigating weeks

- Tap **← Prev** or **Next →** to move one week at a time
- Tap **📅 Pick Week** or the date range in the header to open the calendar and jump to any week
- Swipe left or right on the main screen on iPhone to change weeks
- Tap **This Week** to return to the current week instantly
- Tap any week in the **Monthly Overview** panel to jump directly to it

---

## OneDrive & Azure Setup

The Save to Excel feature uses the **Microsoft Graph API** with **MSAL.js** for secure browser-based authentication. No client secret is stored in the app or transmitted — login is handled entirely via Microsoft's secure popup flow.

### Prerequisites

- A Microsoft Azure account
- An active OneDrive account linked to that Azure tenant
- The folder `/Double Sided ISCM/Payroll/` will be created automatically on first save if it does not exist

### Step 1 — Register an Azure App

1. Go to [portal.azure.com](https://portal.azure.com)
2. Navigate to **Azure Active Directory** → **App registrations**
3. Click **New registration**
4. Enter a name (e.g. `Payroll App`)
5. Under **Supported account types**, select **Accounts in this organizational directory only** or **Accounts in any organizational directory** depending on your setup
6. Click **Register**

### Step 2 — Note your credentials

From the app's **Overview** page, copy:
- **Application (client) ID**
- **Directory (tenant) ID**

These are the only two values needed. The client secret is not used.

### Step 3 — Configure the Redirect URI

1. In your app registration, go to **Authentication**
2. Click **Add a platform** → **Single-page application**
3. Enter your GitHub Pages URL as the redirect URI:
   ```
   https://<your-username>.github.io/<repository-name>
   ```
4. Click **Configure**

### Step 4 — Set API Permissions

1. Go to **API permissions** → **Add a permission**
2. Select **Microsoft Graph** → **Delegated permissions**
3. Add the following permissions:
   - `Files.ReadWrite`
   - `User.Read`
4. Click **Grant admin consent** if you are an administrator, or request consent from your administrator

### Step 5 — Configure the app

1. Open the Payroll App and tap the **⚙️ gear icon** in the header
2. Paste your **Client ID** and **Tenant ID** into the respective fields
3. Tap **🔗 Sign in with Microsoft**
4. Complete the Microsoft login in the popup window
5. The gear icon will turn green confirming a successful connection

Credentials are saved locally to your device and persist across sessions. You will only need to sign in again if your session expires.

---

## Technical Architecture

| Concern | Implementation |
|---|---|
| Runtime | Single HTML file, vanilla JavaScript, no build step |
| Styling | CSS custom properties, dark/light theme, mobile-first |
| Fonts | Syne (headings), DM Mono (data) via Google Fonts |
| Data storage | Browser `localStorage` — persists per device, per origin |
| Authentication | MSAL.js 2.x (`@azure/msal-browser`) via CDN, popup flow |
| Excel integration | Microsoft Graph API — `Files.ReadWrite` scope, range PATCH |
| Offline support | Service Worker registered via Blob URL, caches Google Fonts |
| PWA | Web App Manifest (inline data URI), apple-touch-icon, standalone display mode |
| iOS support | `viewport-fit=cover`, `env(safe-area-inset-*)`, `-webkit-fill-available` |
| Swipe navigation | Native touch events, horizontal gesture detection |
| CSV export | Blob + Object URL download, iOS Web Share API with file attachment |

### Data flow — Save to Excel

```
User taps "Save to Excel"
  → MSAL acquireTokenSilent (falls back to popup if expired)
  → Graph: GET /me/drive/root:/Double Sided ISCM/Payroll/Payroll_Master.xlsx
      → 404: create folder structure, upload blank .xlsx, create "Payroll" sheet + header row
      → 200: open existing file
  → Graph: GET usedRange → determine next empty row
  → Graph: PATCH range → append this week's rows
  → Toast confirmation
```

### File structure

```
repository/
├── index.html        # Entire application — HTML, CSS, JavaScript in one file
└── README.md         # This document
```

---

## Data & Privacy

- All payroll data is stored **locally on your device** in `localStorage`
- No data is sent to any external server other than Microsoft OneDrive when you explicitly tap **Save to Excel**
- Microsoft authentication tokens are managed by MSAL.js and stored in `localStorage` on your device
- Your Azure Client ID and Tenant ID are stored locally in `localStorage` — they are not secret values and are safe to store client-side
- Clearing Safari's website data in iPhone Settings will erase all locally stored payroll data — export to CSV or Excel regularly as a backup
- The app has no analytics, no tracking, and no third-party data collection of any kind

---

## License

Internal tool — Double Sided ISCM LLC. All rights reserved.