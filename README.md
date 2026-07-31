# ERA Suite

**A complete work-log system for ERA Solutions' Technical Office — built as a single self-contained HTML app, with no backend and no build step.**

ERA Suite replaces paper-based daily logging with a fast, structured, Odoo-style interface for tracking technical office work, on-site installations, and maintenance intake — with professional PDF exports, per-site history, and optional cross-device cloud sync.

---

## 🔗 Live

| Edition | Link | Who it's for |
|---|---|---|
| **Full Suite** | [`/`](https://abdellrahmanahmed.github.io/era-suite/) | Technical Office — all four tracks |
| **Installations Team** | [`/install.html`](https://abdellrahmanahmed.github.io/era-suite/install.html) | Field engineers — installations only, shared team workspace |

---

## 📦 Two Editions in This Repo

This repository ships **two builds from the same codebase**, each tailored to a different team:

### `index.html` — Full Suite
The complete tool for the Technical Office. Includes all four work tracks (Technical Office, Installations, Maintenance, Other), personal settings, and an optional private cloud sync workspace.

### `install.html` — Installations Team Edition
A focused, single-track build for field engineers:
- **Password-gated login** — each engineer signs in with their name, phone, and an assigned access code (SHA-256 hashed, no plaintext credentials in the source)
- **Single track** — installations only, nothing else cluttering the flow
- **Shared team workspace** — every engineer's entries sync automatically to one common Firebase workspace, so the whole team's activity is visible in one place
- **Reporter attribution** — every entry and report is tagged with the engineer who filed it
- **Org-gated site management** — adding a new site or linking a local sync folder requires a separate organization-level password (kept by the Technical Office), so field engineers can only log against approved sites
- **Site files** — every site automatically builds its own history: contacts, network credentials, device log, fault history, and open items

---

## ✨ Features

### 📊 Dashboard
- 8-week activity chart
- Monthly KPIs: entries, active days, open items, sites touched
- Quick access to recent entries

### 📝 Work Tracks
Each track has its own tailored, step-by-step entry flow:

| Track | Purpose |
|---|---|
| 🏢 **Technical Office** | AutoCAD drawings, BOQs, quotations, design reviews, internal tools |
| 🔧 **Installations** | Full site-visit reports — devices, network info, programming, scenarios, GPS location |
| 🛠 **Maintenance** | Structured intake with itemized units, fault codes, auto-generated job codes, and a print-ready PDF |
| 📌 **Other** | Meetings, training, admin work — anything that doesn't fit the above |

### 🏢 Site Files
Every project/site automatically builds its own living record:
- Client info, network credentials, and location — learned automatically from entries
- Full visit timeline across all tracks
- Installed device history
- Fault/maintenance history
- Open items and accumulated notes
- One-click PDF export per site
- *(Installations edition)* Odoo-style site list with freshness indicators, open-item counts, and sorting

### 🧾 Reports
- Flexible period reports (week / month / quarter / half-year / custom range)
- Filterable by track
- Summary or detailed view
- Auto-aggregated: workload estimate, task counts, systems breakdown, most common faults
- Professional print-to-PDF layout with the ERA letterhead
- CSV/Excel export of the full log

### 🖨 Professional PDF Output
- Maintenance intake reports with itemized tables, terms & conditions, and signature blocks
- QR code for the site location
- Branded header/footer with company contact details
- Fully theme-aware — the PDF matches whichever color theme is active

### 🎨 Theming
- Multiple built-in color themes (light and dark)
- Full custom color picker (applies to both the UI and the PDF output)

### ☁️ Cross-Device Sync (optional)
- Powered by Firebase Firestore + Anonymous Authentication
- Last-write-wins conflict resolution using per-entry timestamps
- Works fully offline without it — sync is opt-in on the full suite, always-on for the shared team workspace in the installations edition

### 🗂 Local Folder Backup (Chrome/Edge desktop)
- Uses the File System Access API to write every entry as a `.txt` file into an organized folder structure on disk, split by track
- Automatic JSON backup alongside it

### 📲 WhatsApp-Native Output
- Every entry and report is formatted for direct sharing — copy, send to yourself, or share to Telegram

---

## 🛠 Tech Stack

- **Zero build step** — self-contained HTML files, no dependencies to install
- Vanilla JavaScript (ES2017+), no framework
- [Tajawal](https://fonts.google.com/specimen/Tajawal) for Arabic typography
- [QRCode.js](https://github.com/davidshimjs/qrcodejs) for location QR codes (CDN)
- [Firebase](https://firebase.google.com/) (Firestore + Auth) for optional cloud sync (CDN, loaded on demand)
- Web Crypto API (`SHA-256`) for hashed access codes in the installations edition
- `localStorage` for offline-first local persistence
- File System Access API for local folder sync (Chromium browsers)

No backend, no bundler, no npm install. Open either file and it works.

---

## 🚀 Getting Started

### Option 1 — Just open it
Download `index.html` or `install.html` and open it in any modern browser (Chrome or Edge recommended for full folder-sync support).

### Option 2 — Host it (GitHub Pages)
This repo is served directly via GitHub Pages — see the links at the top of this README.

### Enabling cloud sync (full suite)
1. Create a free project at [console.firebase.google.com](https://console.firebase.google.com)
2. Enable **Firestore Database** (Production mode)
3. Enable **Anonymous** sign-in under **Authentication → Sign-in method**
4. Copy your Web app's `firebaseConfig` object
5. Publish these Firestore rules:
   ```
   rules_version = '2';
   service cloud.firestore {
     match /databases/{db}/documents {
       match /workspaces/{ws}/entries/{doc} {
         allow read, write: if request.auth != null;
       }
     }
   }
   ```
6. In the app, go to **☁️ Sync**, paste your config, set a private workspace code, and enable sync on each device you use.

The installations edition ships with its Firebase config and shared workspace pre-configured — engineers just log in with an assigned access code.

---

## 📂 Data & Privacy

- All data is stored locally in the browser by default — nothing leaves your device unless cloud sync is enabled.
- Cloud sync data is scoped to a private workspace code and protected by Firestore auth rules.
- Access codes in the installations edition are stored as SHA-256 hashes, never in plaintext.
- No analytics, no third-party tracking, no ads.

---

## 📄 License

This project is provided as-is for internal use at ERA Solutions. Feel free to fork and adapt for your own workflow.

---

## ✍️ Author

**م. عبدالرحمن أحمد عبدالدايم**
*Eng. Abdelrahman Ahmed Abdeldayem*
Smart Home & Automation Engineer — Technical Office, ERA Solutions

🔗 [linkedin.com/in/engabdaim](https://www.linkedin.com/in/engabdaim)
🌐 [erasolutions.org](https://erasolutions.org/)

---

<p align="center">Built with ⚡ for ERA Solutions' Technical Office</p>
