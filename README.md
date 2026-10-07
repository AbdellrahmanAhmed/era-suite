# ERA Suite

**A work-log, field-reporting and site-survey system for ERA Solutions' Technical Office — self-contained HTML apps, no backend, no build step.**

ERA Suite replaces paper-based logging with fast, structured, Odoo-style tools for tracking technical office work, on-site installations, maintenance intake and pre-sales site surveys — with professional PDF exports, per-site history, role-based access, and real-time cloud sync.

> **Version 2.0** · Last updated 2026-10-07 · [Changelog](CHANGELOG.md) · [Older versions](docs/archive/)

---

## 🔗 Live

| Tool | Link | Who it's for |
|---|---|---|
| **Work Suite** | [`/`](https://abdellrahmanahmed.github.io/era-suite/) | Personal work log — any Google account, private space per user |
| **Installations** | [`/install.html`](https://abdellrahmanahmed.github.io/era-suite/install.html) | Field team — shared workspace, invite-only |
| **Site Survey** | [`/survey.html`](https://abdellrahmanahmed.github.io/era-suite/survey.html) | Pre-sales surveys — zone-by-zone device take-off |

---

## 📦 The Three Tools

### `index.html` — Work Suite (personal)

A full personal work log across four tracks. Sign in with any Google account; **each account gets a completely private space** (`users/{uid}`) that no one else can read.

| Track | Covers |
|---|---|
| 🏢 Technical Office | AutoCAD drawings, BOQs, quotations, design reviews, internal tools, web/app development, self-study, R&D |
| 🔧 Installations | Full site-visit reports — devices, network, programming, scenarios, GPS |
| 🛠 Maintenance | Itemized intake with fault codes, auto job codes, print-ready PDF |
| 📌 Other | Meetings, training, admin |

Technical-office entries also record **technology platforms** (Home Assistant, Node-RED, ESPHome, MQTT, Zigbee2MQTT, KNX ETS, Tuya IoT, Matter/Thread, Python, Flutter, Web, Firebase, Docker, AI/LLM), a learning source, and a project/repo link — so period reports show where the time actually went.

Projects can be **deleted** by their owner, and period reports can be **filtered to a single project**.

### `install.html` — Installations (team)

A focused, single-track build for the field team, backed by one shared workspace with **server-enforced roles**.

| Capability | Owner | Manager | Engineer |
|---|:---:|:---:|:---:|
| Manage team members | ✅ | ❌ | ❌ |
| Create / edit sites · open & close · maintenance flag | ✅ | ✅ | ❌ |
| Upload site documents | ✅ | ✅ | ❌ |
| Delete site documents | ✅ | ❌ | ❌ |
| Add / delete site notes | ✅ | ✅ | ❌ |
| Link a local sync folder | ✅ | ✅ | ❌ |
| Log visits, view sites & documents, export reports | ✅ | ✅ | ✅ |

- **Google Sign-In only.** An email that isn't in the member directory is refused outright — no data is ever fetched.
- **Member directory.** The owner registers each engineer's email, name, phone and role; names and phones are pulled automatically.
- **Sites are managed, not improvised.** Engineers pick from the sites the office opened — they can't invent one.
- **Open / closed status** plus an independent **🛠 maintenance flag**: a *closed* site with the maintenance flag still accepts visits, but the report type is locked to **تقرير صيانة موقع**.
- **Closed sites suppress open items** everywhere — counts, dashboard KPIs, site files and period reports.
- **Site data auto-fills** into each visit and stays editable per visit without overwriting the master record.
- **📎 Site documents** — PDFs and images uploaded to Firebase Storage, grouped and filterable by document type.
- **📝 Site notes** — multiple dated notes per site, each tagged and attributed to its author.
- **📦 Site store** — a per-visit inventory of what's left in the site's store; the site file shows the latest snapshot.
- **Web-formatted feedback** — every visit report renders as structured HTML (headed sections, lists, status bar) with a one-tap switch back to the raw WhatsApp text.

### `survey.html` — Site Survey (standalone)

A pre-sales / design survey tool for capturing a complete smart-home take-off, zone by zone.

- **4-step wizard** — client & unit details → zone selection → per-zone specification → report
- **Unit presets** — apartment, duplex, villa (3 or 4 floors) auto-expand the zone list; custom zones can be added
- **Per zone:** lighting lines, wall boxes, bedside panels, shutters, curtains, curtain-track metres, AC units, sensors checklist, touch screen, voice control, network solutions, and a full sound-system block (speaker type, count, amplifier, zone merging)
- **Summary table with aggregated totals** across every zone
- **Landscape A4 PDF export** with the ERA letterhead and signature blocks for the engineer and the client
- **Local drafts and a saved-surveys history** stored in the browser

> ⚠️ This tool is **standalone**: it uses browser storage only and is not connected to Google Sign-In or Firebase. Surveys live on the device that created them and are not shared with the team. Export the PDF for anything that must be kept.

---

## ✨ Shared Features

### 📊 Dashboard
8-week activity chart · monthly KPIs (entries, active days, open items, sites touched) · recent entries

### 🏢 Site / Project Files
Contacts, credentials, written address and map location · full visit timeline · installed-device history · fault history · open items · documents · notes · latest store snapshot · one-click PDF · Odoo-style list with freshness dots, open-item counts, status filters and sorting

### 🧾 Reports
Period reports (week / month / quarter / half-year / custom), filterable by track and project · auto-aggregated workload estimate, task counts, systems and technology breakdown, most frequent faults · branded print-to-PDF · CSV/Excel export

### 🖨 PDF Output
Maintenance intake reports with itemized tables, terms & conditions and signature blocks · QR code for the site location · branded header/footer · theme-aware

### 🎨 Theming
Built-in light and dark themes plus a custom colour picker, applied to both UI and PDF output

### ☁️ Sync
Firebase Firestore with Google Auth · last-write-wins per entry · **true deletes** that propagate to every device without resurrection · site registry, documents and notes synced team-wide · resilient to partial failures · **🩺 8-step connection diagnostic** that names the exact failing step

### 🧭 Navigation & Access
Browser **back button and swipe-back** work throughout, including step-by-step inside the entry wizard · **in-app browser detection** (WhatsApp, Instagram, Facebook) with a clear "open in browser" prompt and an automatic redirect fallback for Google Sign-In

### 🗂 Local Folder Backup (Chrome/Edge desktop)
File System Access API writes every entry as a `.txt` into an organised folder tree split by track, with a rolling JSON backup

### 📲 WhatsApp-Native Output
Every entry and report is formatted for direct sharing — copy, send to yourself, or share to Telegram

---

## 🔐 Security Model

Authorization is enforced by **Firestore and Storage security rules on the server**, not in the browser. Client-side source is fully visible (as with any web app) — without an authorized Google account, no data can be read or written.

```
users/{uid}/**                 → only that signed-in user
workspaces/{ws}/entries/**     → any member of the workspace
workspaces/{ws}/meta/**        → read: members · write: managers and owner
workspaces/{ws}/docs/**        → read: members · write: managers and owner
workspaces/{ws}/sitenotes/**   → read: members · write: managers and owner
workspaces/{ws}/members/**     → read: self, managers, owner · write: owner only
storage: workspaces/{ws}/*     → read & upload: signed-in · delete: owner only
everything else                → denied
```

The Firebase `apiKey` is public by design (Google intends it to ship in client code); the rules above are what actually protect the data.

---

## 🛠 Tech Stack

- **Zero build step** — self-contained HTML files, nothing to install
- Vanilla JavaScript (ES2017+), no framework (survey tool uses Tailwind CDN)
- [Tajawal](https://fonts.google.com/specimen/Tajawal) / [Cairo](https://fonts.google.com/specimen/Cairo) for Arabic typography
- [QRCode.js](https://github.com/davidshimjs/qrcodejs) for location QR codes
- [html2pdf.js](https://github.com/eKoopmans/html2pdf.js) for the survey PDF
- [Firebase](https://firebase.google.com/) Firestore · Storage · Google Auth (loaded on demand from CDN)
- `localStorage` for offline-first local persistence
- File System Access API for local folder sync (Chromium)

---

## 🚀 Setup

### Run it
Open any file in a modern browser (Chrome or Edge for full folder-sync support), or use the hosted links above.

### Configure Firebase (once)
1. Create a project at [console.firebase.google.com](https://console.firebase.google.com)
2. **Firestore Database** → Create (Production mode)
3. **Storage** → Get started (Production mode, same region)
4. **Authentication → Sign-in method** → enable **Google**
5. **Authentication → Settings → Authorized domains** → add your GitHub Pages domain
6. **Firestore → Rules** → publish [`firestore-rules.txt`](firestore-rules.txt)
7. **Storage → Rules** → publish [`storage-rules.txt`](storage-rules.txt)
8. Open the Installations edition as the owner → **👥 Team** → add each engineer's email, name, phone and role

The Firebase config is baked into the files; engineers only sign in with Google. The survey tool needs no setup at all.

---

## 📂 Data & Privacy

- Local-first: everything works from the browser's own storage, sync layered on top
- Personal spaces are isolated per Google account and unreadable by anyone else
- Team data is scoped to the member directory and enforced by security rules
- Survey data never leaves the device unless exported as PDF
- No analytics, no third-party tracking, no ads

---

## 🗃 Versioning

Current documentation is this file. Previous major versions are archived under [`docs/archive/`](docs/archive/), and every change is listed in [`CHANGELOG.md`](CHANGELOG.md).

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
