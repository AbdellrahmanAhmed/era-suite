# ERA Suite

**A work-log and field-reporting system for ERA Solutions' Technical Office — two self-contained HTML apps, no backend, no build step.**

ERA Suite replaces paper-based logging with a fast, structured, Odoo-style interface for tracking technical office work, on-site installations, and maintenance intake — with professional PDF exports, per-site history, role-based access, and real-time cloud sync.

---

## 🔗 Live

| Edition | Link | Who it's for |
|---|---|---|
| **Work Suite** | [`/`](https://abdellrahmanahmed.github.io/era-suite/) | Personal work log — any Google account, private space per user |
| **Installations** | [`/install.html`](https://abdellrahmanahmed.github.io/era-suite/install.html) | Field team — shared workspace, invite-only |

---

## 📦 Two Editions

### `index.html` — Work Suite (personal)

A full personal work log across four tracks. Sign in with any Google account; **each account gets a completely private space** (`users/{uid}`) that no one else can read.

**Tracks**

| Track | Covers |
|---|---|
| 🏢 Technical Office | AutoCAD drawings, BOQs, quotations, design reviews, internal tools, **web/app development, self-study, R&D** |
| 🔧 Installations | Full site-visit reports — devices, network, programming, scenarios, GPS |
| 🛠 Maintenance | Itemized intake with fault codes, auto job codes, print-ready PDF |
| 📌 Other | Meetings, training, admin |

Technical-office entries can also record **technology platforms** (Home Assistant, Node-RED, ESPHome, MQTT, Zigbee2MQTT, KNX ETS, Tuya IoT, Matter/Thread, Python, Flutter, Web, Firebase, Docker, AI/LLM), a learning source, and a project/repo link — so period reports show where the time actually went.

### `install.html` — Installations (team)

A focused, single-track build for the field team, backed by one shared workspace with **server-enforced roles**.

| Capability | Owner | Manager | Engineer |
|---|:---:|:---:|:---:|
| Manage team members | ✅ | ❌ | ❌ |
| Create / edit / delete sites | ✅ | ✅ | ❌ |
| Open or close a site | ✅ | ✅ | ❌ |
| Link a local sync folder | ✅ | ✅ | ❌ |
| Log visits, view sites, export reports | ✅ | ✅ | ✅ |

- **Google Sign-In only.** An email that isn't in the member directory is refused outright — the app shows an "unauthorized" screen and no data is ever fetched.
- **Member directory.** The owner registers each engineer's email, name, phone, and role. Names and phone numbers are pulled from the directory automatically, so nobody types them.
- **Sites are managed, not improvised.** Engineers pick from the list of sites the office has opened; they can't invent one. Sites carry an **open / closed** status — a closed site can't receive new visits.
- **Site data auto-fills** into each visit (responsible engineer, phone, SSID, password, location) and stays editable per visit — editing it in a report never overwrites the master site record.
- **Reporter attribution.** Every entry and report is tagged with the engineer who filed it.

---

## ✨ Features

### 📊 Dashboard
8-week activity chart · monthly KPIs (entries, active days, open items, sites touched) · recent entries

### 🏢 Site Files
Every site builds its own living record, automatically:
- Contacts, network credentials, written address and map location
- Full visit timeline
- Installed-device history and fault history
- Open items and accumulated notes
- One-click PDF export
- Odoo-style list with freshness indicators, open-item counts, status filters, and sorting

### 🧾 Reports
- Period reports (week / month / quarter / half-year / custom), filterable by track
- Auto-aggregated: workload estimate, task counts, systems and technology breakdown, most frequent faults
- Summary or detailed view
- Branded print-to-PDF layout
- CSV/Excel export of the whole log

### 🖨 PDF Output
Maintenance intake reports with itemized tables, terms & conditions, and signature blocks · QR code for the site location · branded header/footer · theme-aware (the PDF follows the active colour theme)

### 🎨 Theming
Multiple built-in light and dark themes plus a full custom colour picker, applied to both the UI and PDF output.

### ☁️ Sync
- Firebase Firestore, signed in with Google
- Last-write-wins per entry via timestamps
- **True deletes** — removing an entry deletes the cloud document and propagates the removal to every device, without resurrection on the next sync
- Site registry (approved sites and their data) syncs to the whole team
- Resilient: one failed write no longer aborts the whole sync
- **🩺 Connection diagnostic** — an 8-step check (config → SDK → init → auth → write → read → delete) that names the exact failing step

### 🗂 Local Folder Backup (Chrome/Edge desktop)
File System Access API writes every entry as a `.txt` into an organised folder tree split by track, with a rolling JSON backup beside it.

### 📲 WhatsApp-Native Output
Every entry and report is formatted for direct sharing — copy, send to yourself, or share to Telegram.

---

## 🔐 Security Model

Authorization is enforced by **Firestore security rules on the server**, not in the browser. Client-side source is fully visible (as with any web app) and that's fine — without an authorized Google account, no data can be read or written.

```
users/{uid}/**              → only that signed-in user
workspaces/{ws}/entries/**  → any member of the workspace
workspaces/{ws}/meta/**     → read: members · write: managers and owner
workspaces/{ws}/members/**  → read: self, managers, owner · write: owner only
everything else             → denied
```

The Firebase `apiKey` is public by design (Google intends it to ship in client code); the rules above are what actually protect the data.

---

## 🛠 Tech Stack

- **Zero build step** — two self-contained HTML files, nothing to install
- Vanilla JavaScript (ES2017+), no framework
- [Tajawal](https://fonts.google.com/specimen/Tajawal) for Arabic typography
- [QRCode.js](https://github.com/davidshimjs/qrcodejs) for location QR codes
- [Firebase](https://firebase.google.com/) Firestore + Google Auth (loaded on demand from CDN)
- `localStorage` for offline-first local persistence
- File System Access API for local folder sync (Chromium)

---

## 🚀 Setup

### Run it
Open either file in a modern browser (Chrome or Edge for full folder-sync support), or use the hosted links above.

### Configure Firebase (once)
1. Create a project at [console.firebase.google.com](https://console.firebase.google.com)
2. **Firestore Database** → Create (Production mode)
3. **Authentication → Sign-in method** → enable **Google**
4. **Authentication → Settings → Authorized domains** → add your GitHub Pages domain
5. **Firestore → Rules** → publish the rules from [`firestore-rules.txt`](firestore-rules.txt)
6. Open the Installations edition as the owner → **👥 Team** → add each engineer's email, name, phone, and role

The Firebase config is baked into both files; engineers only sign in with Google.

---

## 📂 Data & Privacy

- Local-first: everything works from the browser's own storage, sync layered on top
- Personal spaces are isolated per Google account and unreadable by anyone else
- Team data is scoped to the member directory and enforced by security rules
- No analytics, no third-party tracking, no ads

---

## 📄 License

Provided as-is for internal use at ERA Solutions. Fork and adapt freely for your own workflow.

---

## ✍️ Author

**م. عبدالرحمن أحمد عبدالدايم**

*Eng. ABDELRAHMAN AHMED ABDELDAIM*
Smart Home & Automation Engineer — Technical Office, ERA Solutions

🔗 [linkedin.com/in/engabdaim](https://www.linkedin.com/in/engabdaim)
🌐 [erasolutions.org](https://erasolutions.org/)

---

<p align="center">Built with ⚡ for ERA Solutions' Technical Office</p>
