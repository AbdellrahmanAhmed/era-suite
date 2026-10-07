# Changelog

All notable changes to **ERA Suite** are recorded here, newest first.
Documentation versions are archived under [`docs/archive/`](docs/archive/).

---

## [2.0] — 2026-10-07

### Added
- **`survey.html` — Site Survey tool.** Standalone 4-step wizard for pre-sales smart-home take-off: client & unit details → zone selection → per-zone specification → summary report with landscape A4 PDF export. Unit presets (apartment, duplex, villa 3/4 floors), per-zone lighting lines, wall boxes, bedside panels, shutters, curtains, track metres, AC, sensors, touch screen, voice control, network solutions and a full sound-system block. Local drafts + saved-surveys history.
- **📎 Site documents** (`install.html`) — one **upload** button, then pick the document type. PDFs and images stored in Firebase Storage, grouped and filterable by type (AutoCAD layout, electrical drawing, device catalogue, contract/quotation, report, operating manual, other). 20 MB limit.
- **📝 Site notes** (`install.html`) — multiple dated notes per site, each tagged (general, alert, site access, client contact, electrical, network) and attributed to its author. Owner and manager only.
- **📦 Site store** — per-visit inventory of what is left in the site's store; the site file shows the latest snapshot with its date.
- **Web-formatted feedback view** — every visit report renders as structured HTML (headed sections, lists, status bar) with a one-tap switch back to the raw WhatsApp text.
- **🛠 Maintenance flag** on sites, independent of open/closed status. A *closed* site carrying the flag still accepts visits, but the report type is locked to **تقرير صيانة موقع**.
- **Back-button and swipe-back navigation** throughout both apps, including step-by-step inside the entry wizard.
- **In-app browser handling** — WhatsApp / Instagram / Facebook browsers are detected, warned about, offered a copy-link, and fall back to `signInWithRedirect` for Google Sign-In.
- **Project deletion and per-project report filter** in the personal Work Suite.
- `storage-rules.txt` — Firebase Storage security rules (read & upload: any signed-in account; delete: owner only).

### Changed
- "مواقع" renamed to **"مشاريع"** throughout the personal Work Suite.
- **Closed sites suppress open items** everywhere — counts, dashboard KPIs, site files and period reports.
- Site documents: upload allowed for owner **and** manager; **delete restricted to the owner** (`a.ahmed.abdaim@gmail.com`).
- Sync is now resilient — one failed write no longer aborts the whole run (`Promise.allSettled`).
- Site registry (approved sites and their data) now syncs to the whole team through `meta/site-registry`.
- New technical-office entry kinds: web/app development, self-study, R&D, server & home-automation automation.

### Fixed
- **Shared local storage between editions** — `install.html` used the same `localStorage` key as the personal suite on the same GitHub Pages origin, so each app read and overwrote the other's data. The team edition now uses its own isolated key.
- **`[invalid-argument] … id is reserved`** — Firestore rejects document IDs matching `__*__`. The site registry and diagnostic documents were renamed, which also fixed the team workspace never being created.
- **"تم المزامنة وتنزيل 3" with nothing changing** — deleted tombstones were counted as pulled entries, and the view wasn't re-rendered after a sync.
- **Sites with no visits were invisible and uneditable** — the site index was built from entries only, so a newly created site couldn't be edited until a report existed on it.
- **Site store showed the oldest snapshot** instead of the newest (the comparison used the formatted date, not the raw timestamp).
- **PDF printing broken in both apps** — the fixed-position sign-in overlay wasn't hidden in `@media print`. Print CSS now hides the overlay, sidebar and chrome, forces the printable sheet to static flow, and adds page-break rules.
- **Signed-in user's name replaced by their email** — the auth listener was re-registered on every Firebase init and re-fired with an empty directory record for the owner.
- **Crash on entries with no date** — defensive guards added to every date formatter, with a default applied on load.
- Engineer edits to site data no longer overwrite the office's master site record (blank fields are filled, existing ones are left alone).

### Documentation
- README rewritten for three tools; archived the previous version as [`docs/archive/README-v1.0.md`](docs/archive/README-v1.0.md).
- Role table expanded to split document upload from document delete.
- Security-model block now covers `docs/`, `sitenotes/` and the Storage paths.
- Firebase setup expanded to 8 steps, including Storage and its rules.

---

## [1.0] — 2026-09

### Added
- **`index.html` — Work Suite (personal).** Four tracks (technical office, installations, maintenance, other), WhatsApp-native output, period reports, site files, maintenance PDF, theming, local folder backup, Firestore sync with per-user private spaces.
- **`install.html` — Installations (team).** Single-track field build on one shared workspace with server-enforced roles (owner / manager / engineer), member directory, managed site list with open/closed status, reporter attribution.
- **Google Sign-In** replacing the earlier local password gate, with `firestore-rules.txt` enforcing all authorization on the server.
- **🩺 Connection diagnostic** — an 8-step check (config → SDK → init → auth → write → read → delete) that names the exact failing step.
- Odoo-style UI, theme colour picker, exact reproduction of the maintenance intake PDF, QR code for the site location.
- Initial README.

---

[2.0]: https://github.com/abdellrahmanahmed/era-suite/releases/tag/v2.0
[1.0]: https://github.com/abdellrahmanahmed/era-suite/releases/tag/v1.0
